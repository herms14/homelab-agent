# Deploy New

Generate deployment code for new homelab services (Terraform, Ansible, Docker Compose).

## Instructions

Generate infrastructure-as-code for deploying new services to the homelab.

### Data Sources

Read these for templates and patterns:
- `07 HomeLab Things/Claude Managed Homelab/05 - Terraform Configuration.md` - TF patterns
- `07 HomeLab Things/Claude Managed Homelab/06 - Ansible Automation.md` - Ansible patterns
- `07 HomeLab Things/Claude Managed Homelab/15 - New Service Onboarding Guide.md` - Workflow
- `07 HomeLab Things/Claude Managed Homelab/22 - Service Onboarding Workflow.md` - Automation
- `07 HomeLab Things/Claude Managed Homelab/09 - Traefik Reverse Proxy.md` - Routing
- `07 HomeLab Things/Claude Managed Homelab/14 - Authentik Google SSO Setup.md` - Auth

### Deployment Types

1. **Docker Service** - Container on existing Docker host
2. **New VM + Docker** - Fresh VM with Docker service
3. **LXC Container** - Lightweight container on Proxmox
4. **Kubernetes Deployment** - K8s workload

### Interactive Wizard

When run without arguments, ask:

```
🚀 New Service Deployment Wizard

1. What's the service name? [name]
2. Deployment type?
   a) Docker on existing host
   b) New VM with Docker
   c) LXC container
   d) Kubernetes
3. Resource requirements?
   - CPU cores: [1-8]
   - RAM (GB): [1-32]
   - Disk (GB): [10-500]
4. Needs external access? [y/n]
5. Authentication method?
   a) Authentik SSO
   b) Native auth
   c) None (internal only)
```

### Output Format

```markdown
# 🚀 Deploy New Service: [Service Name]

**Type**: [Docker/VM/LXC/K8s]
**Target**: [Host/Node]
**IP**: [Suggested IP]
**URL**: https://[name].hrmsmrflrii.xyz

---

## Step 1: Infrastructure (if VM/LXC needed)

### Terraform Configuration

```hcl
# Add to main.tf or create modules/[service]/main.tf

module "[service]_vm" {
  source = "./modules/linux-vm"

  vm_name     = "[service]"
  vmid        = [next_available_vmid]
  target_node = "[suggested_node]"

  cores   = [X]
  memory  = [X] * 1024  # MB
  disk_size = "[X]G"

  ip_address = "[suggested_ip]/24"
  gateway    = "192.168.20.1"

  ssh_keys = var.ssh_public_key

  tags = ["docker", "[service]", "automated"]
}
```

### Apply Terraform

```bash
cd /path/to/terraform
terraform plan
terraform apply
```

---

## Step 2: Docker Compose

```yaml
# docker-compose.[service].yml

version: "3.8"

services:
  [service]:
    image: [image]:[tag]
    container_name: [service]
    restart: unless-stopped

    environment:
      - TZ=Asia/Manila
      - PUID=1000
      - PGID=1000

    volumes:
      - /srv/[service]/config:/config
      - /srv/[service]/data:/data

    ports:
      - "[port]:[port]"

    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.[service].rule=Host(`[service].hrmsmrflrii.xyz`)"
      - "traefik.http.routers.[service].entrypoints=websecure"
      - "traefik.http.routers.[service].tls.certresolver=letsencrypt"
      - "traefik.http.services.[service].loadbalancer.server.port=[port]"

    networks:
      - traefik-net

networks:
  traefik-net:
    external: true
```

### Deploy Container

```bash
# Create directories
sudo mkdir -p /srv/[service]/{config,data}
sudo chown -R 1000:1000 /srv/[service]

# Deploy
docker-compose -f docker-compose.[service].yml up -d

# Verify
docker logs -f [service]
```

---

## Step 3: Traefik Routing

### Dynamic Configuration

Add to Traefik dynamic config or use labels above.

```yaml
# If using file provider
http:
  routers:
    [service]:
      rule: "Host(`[service].hrmsmrflrii.xyz`)"
      entryPoints:
        - websecure
      service: [service]
      tls:
        certResolver: letsencrypt
      middlewares:
        - authentik@file  # If using Authentik

  services:
    [service]:
      loadBalancer:
        servers:
          - url: "http://[ip]:[port]"
```

---

## Step 4: Authentik Integration (if needed)

### Create Application

1. Go to https://auth.hrmsmrflrii.xyz/if/admin/
2. Applications → Create
3. Settings:
   - Name: [Service Name]
   - Slug: [service]
   - Provider: Create new Proxy Provider
   - External Host: https://[service].hrmsmrflrii.xyz

### Proxy Provider Settings

- Authorization flow: default-provider-authorization-implicit-consent
- Forward auth mode: Single application
- External host: https://[service].hrmsmrflrii.xyz

---

## Step 5: DNS (if not using wildcard)

Add to Pi-hole or Cloudflare:
```
[service].hrmsmrflrii.xyz → [Traefik IP]
```

---

## Step 6: Update Documentation

### Add to Deployed Services

Add this row to `07 - Deployed Services.md`:

```markdown
| [Service Name] | https://[service].hrmsmrflrii.xyz | Docker | [port] | Authentik |
```

### Add to IP Address Map

Add to `10 - IP Address Map.md`:

```markdown
| [IP] | [service] | Docker Host/VM | [Description] |
```

---

## Step 7: Monitoring

### Add to Uptime Kuma

1. Go to https://uptime.hrmsmrflrii.xyz
2. Add New Monitor
3. URL: https://[service].hrmsmrflrii.xyz
4. Interval: 60s

---

## Verification Checklist

- [ ] Infrastructure deployed (VM/LXC if needed)
- [ ] Container running (`docker ps`)
- [ ] Traefik routing working
- [ ] SSL certificate issued
- [ ] Authentik protection active (if applicable)
- [ ] DNS resolving
- [ ] Uptime monitor added
- [ ] Documentation updated

---

## Rollback

If something goes wrong:

```bash
# Stop container
docker-compose -f docker-compose.[service].yml down

# Remove VM (if created)
terraform destroy -target=module.[service]_vm

# Remove from Traefik config
# Remove from Authentik
# Update documentation
```
```

## Arguments

- `/deploy-new` - Interactive wizard
- `/deploy-new docker [name]` - Docker service only
- `/deploy-new vm [name]` - New VM with service
- `/deploy-new lxc [name]` - LXC container
- `/deploy-new k8s [name]` - Kubernetes deployment
- `/deploy-new [name] --image [image:tag]` - Specify image
- `/deploy-new [name] --port [port]` - Specify port
