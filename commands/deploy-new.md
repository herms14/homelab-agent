# Deploy New

Generate deployment code for new homelab services (Terraform, Ansible, Docker Compose, LXC).

## Instructions

Generate infrastructure-as-code and a documented checklist for deploying a new service to the homelab.

### Data Sources

Read `CLAUDE.md` first: domain, URL pattern, SSO provider, Docker conventions, timezone, SSH user, default VLAN, **Known Gotchas**. Then, using its **Documentation Structure** table, read (skip any that are missing):
- **Automation** - Terraform / Ansible / CI patterns to copy
- **Onboarding** - Your existing new-service workflow
- **Reverse Proxy** - Routing conventions
- **SSO** - Auth setup
- **Proxmox** - Free VMIDs/CTIDs and node placement
- **IP Map** - Next free IP
- **Task Registry** - Make sure nobody else is already deploying this

Match the patterns found in your docs over the generic examples below.

### Before Generating

1. Check the Task Registry. If this deployment is already `🔄 In Progress`, stop and report it. Otherwise mark it `🔄 In Progress`.
2. Pick an IP using the same rules as `/ip-find next`.
3. Pick a node using the same logic as `/capacity plan`.
4. Pick the next free VMID / CTID from the Proxmox doc.

### Deployment Types

1. **Docker Service** - Container on an existing Docker host (VM or LXC)
2. **LXC Container** - Lightweight container on Proxmox (with or without Docker inside)
3. **New VM + Docker** - Fresh VM with Docker service
4. **Kubernetes Deployment** - K8s workload (only if Kubernetes is active)

### Interactive Wizard

When run without arguments, ask:

```
🚀 New Service Deployment Wizard

1. What's the service name? [name]
2. Deployment type?
   a) Docker on existing host
   b) LXC container
   c) New VM with Docker
   d) Kubernetes (if available)
3. Resource requirements?
   - CPU cores: [1-8]
   - RAM (GB): [1-32]
   - Disk (GB): [8-500]
4. Needs external access? [y/n]
5. Authentication method?
   a) SSO (forward auth)
   b) Native auth
   c) None (internal only)
```

### Output Format

````markdown
# 🚀 Deploy New Service: [Service Name]

**Type**: [Docker/LXC/VM/K8s]
**Target**: [Host/Node]
**ID**: [VMID/CTID]
**IP**: [Suggested IP]
**URL**: https://[service].[domain]

---

## Step 1: Infrastructure (if VM/LXC needed)

### Option A: LXC via Proxmox CLI

```bash
pct create [ctid] [storage]:vztmpl/[template].tar.zst \
  --hostname [service]-lxc \
  --cores [X] --memory [X*1024] --swap 512 \
  --rootfs [storage]:[X] \
  --net0 name=eth0,bridge=vmbr0,tag=[vlan],ip=[ip]/24,gw=[gateway] \
  --nameserver [dns] \
  --unprivileged 1 --features nesting=1 \
  --onboot 1 --start 1
```

### Option B: VM via Terraform

```hcl
module "[service]_vm" {
  source = "./modules/linux-vm"

  vm_name     = "[service]"
  vmid        = [next_available_vmid]
  target_node = "[suggested_node]"

  cores     = [X]
  memory    = [X] * 1024  # MB
  disk_size = "[X]G"

  ip_address = "[suggested_ip]/24"
  gateway    = "[gateway]"

  ssh_keys = var.ssh_public_key

  tags = ["docker", "[service]", "automated"]
}
```

```bash
terraform plan
terraform apply
```

---

## Step 2: Docker Compose

```yaml
# /srv/[service]/docker-compose.yml

services:
  [service]:
    image: [image]:[tag]
    container_name: [service]
    restart: unless-stopped

    environment:
      - TZ=[timezone]
      - PUID=1000
      - PGID=1000

    volumes:
      - /srv/[service]/config:/config
      - /srv/[service]/data:/data

    ports:
      - "[port]:[port]"

    logging:
      driver: json-file
      options:
        max-size: "100m"
        max-file: "3"

    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.[service].rule=Host(`[service].[domain]`)"
      - "traefik.http.routers.[service].entrypoints=websecure"
      - "traefik.http.routers.[service].tls.certresolver=letsencrypt"
      - "traefik.http.services.[service].loadbalancer.server.port=[port]"
```

Use labels only if Traefik runs on the same Docker host. Otherwise use the file provider in Step 3.
Log rotation prevents a chatty container from filling the disk.

```bash
sudo mkdir -p /srv/[service]/{config,data}
sudo chown -R 1000:1000 /srv/[service]
cd /srv/[service] && docker compose up -d
docker logs -f [service]
```

---

## Step 3: Reverse Proxy Routing (file provider)

```yaml
http:
  routers:
    [service]:
      rule: "Host(`[service].[domain]`)"
      entryPoints:
        - websecure
      service: [service]
      tls:
        certResolver: letsencrypt
      middlewares:
        - [sso-middleware]  # If using SSO forward auth

  services:
    [service]:
      loadBalancer:
        servers:
          - url: "http://[ip]:[port]"
```

---

## Step 4: SSO Integration (if needed)

1. Open your SSO admin UI (https://auth.[domain])
2. Create a Proxy Provider (forward auth, single application)
   - External host: https://[service].[domain]
3. Create an Application linked to that provider (slug: `[service]`)
4. Assign it to the outpost used by your reverse proxy

---

## Step 5: DNS (if not using a wildcard record)

Add to your local DNS (Pi-hole, AdGuard, router) or public DNS:
```
[service].[domain] → [reverse proxy IP]
```

---

## Step 6: Monitoring

- Add an HTTP(S) monitor for https://[service].[domain] (Uptime Kuma or similar), 60s interval
- Add a Prometheus scrape target if the service exposes metrics

---

## Step 7: Update Documentation (Change Documentation Protocol)

| Doc (role) | Add |
|------------|-----|
| Services | `\| [Service] \| https://[service].[domain] \| [host] \| [port] \| [auth] \|` |
| IP Map | `\| [ip] \| [service] \| [Type] \| [Description] \|` (if a new IP) |
| Proxmox | New VM / LXC row (if created) |
| Changelog | `Added` entry for today |
| Task Registry | Mark the deployment `✅ Completed` |

Update `updated:` frontmatter on every modified doc.

---

## Verification Checklist

- [ ] Infrastructure deployed (VM/LXC if needed)
- [ ] Container running (`docker ps`)
- [ ] Routing working (`curl -I https://[service].[domain]`)
- [ ] TLS certificate issued
- [ ] SSO protection active (if applicable)
- [ ] DNS resolving
- [ ] Monitor added
- [ ] Documentation, Changelog, and Task Registry updated

---

## Rollback

```bash
cd /srv/[service] && docker compose down
pct stop [ctid] && pct destroy [ctid]           # if LXC created (confirm first)
terraform destroy -target=module.[service]_vm   # if VM created (confirm first)
```

Then remove the routing rule and SSO application, and log a `Removed` Changelog entry.
````

Never include real secrets in generated code. Use `${ENV_VAR}` references or `.env` files and tell the user which values to fill in.

## Arguments

- `/deploy-new` - Interactive wizard
- `/deploy-new docker [name]` - Docker service on an existing host
- `/deploy-new lxc [name]` - LXC container
- `/deploy-new vm [name]` - New VM with service
- `/deploy-new k8s [name]` - Kubernetes deployment
- `/deploy-new [name] --image [image:tag]` - Specify image
- `/deploy-new [name] --port [port]` - Specify port
- `/deploy-new [name] --host [host]` - Specify target host or node
