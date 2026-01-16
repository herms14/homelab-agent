# Customization Guide

> How to customize Homelab Agent for your infrastructure

---

## Overview

The Homelab Agent adapts to your infrastructure through:

1. **CLAUDE.md** - Describes your environment
2. **Command files** - Modify behavior
3. **Documentation structure** - What files to read

---

## Customizing CLAUDE.md

This is the most important file. It tells Claude about your infrastructure.

### Basic Template

```markdown
# Homelab Context

## Overview

**Cluster Name**: [YourClusterName]
**Domain**: [yourdomain.xyz]
**Owner**: [Your Name]
**Documentation Location**: [This folder]

## Infrastructure Summary

| Component | Count | Notes |
|-----------|-------|-------|
| Proxmox Nodes | [X] | [Cluster name] |
| Virtual Machines | [X] | Across all nodes |
| Docker Services | [X] | Various hosts |
| Kubernetes Nodes | [X] | [If applicable] |

## Network Architecture

### VLANs

| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 1 | Default | 192.168.1.0/24 | General |
| 10 | Servers | 192.168.10.0/24 | Server VLAN |
| 20 | Homelab | 192.168.20.0/24 | Lab equipment |

### IP Conventions

- Network equipment: .1-.10
- Servers/VMs: .20-.100
- Containers: .100-.150
- DHCP: .200-.254

## Documentation Structure

| File/Folder | Purpose |
|-------------|---------|
| `network.md` | Network architecture |
| `services.md` | Service catalog |
| `ips.md` | IP allocations |
| `proxmox.md` | Proxmox cluster |
| `kubernetes.md` | K8s cluster |
| `troubleshooting.md` | Known issues |
| `changelog.md` | Change log |

## Conventions

### Service URLs
- Pattern: `https://[service].[domain]`
- Example: `https://grafana.yourdomain.xyz`

### Authentication
- Primary: [Authentik/Authelia/None]
- SSO Provider: [Google/Azure/Local]

### Docker
- Compose location: `/srv/[service]/docker-compose.yml`
- Data location: `/srv/[service]/data`
- Config location: `/srv/[service]/config`

### Terraform
- Module location: `./modules/`
- State: [Local/Remote]

### Ansible
- Inventory: `./inventory/hosts.yml`
- Playbooks: `./playbooks/`
```

### Advanced CLAUDE.md

For complex environments, add more sections:

```markdown
## Proxmox Cluster

### Nodes

| Node | IP | Role | CPU | RAM | Storage |
|------|-----|------|-----|-----|---------|
| node01 | 192.168.20.20 | Primary | 24 | 64GB | 1TB NVMe |
| node02 | 192.168.20.21 | Secondary | 20 | 48GB | 500GB SSD |

### Storage Pools

| Pool | Type | Location | Size |
|------|------|----------|------|
| local-lvm | LVM | Node local | Varies |
| nfs-data | NFS | NAS | 4TB |

## Kubernetes Cluster

### Architecture

- Distribution: [kubeadm/k3s/RKE]
- CNI: [Calico/Flannel/Cilium]
- Ingress: [Traefik/Nginx/Contour]

### Nodes

| Name | Role | IP |
|------|------|-----|
| k8s-cp-01 | Control | 192.168.20.50 |
| k8s-w-01 | Worker | 192.168.20.61 |

## Automation

### Terraform Modules

| Module | Purpose |
|--------|---------|
| `linux-vm` | Provision Linux VMs |
| `lxc` | Provision LXC containers |

### Ansible Roles

| Role | Purpose |
|------|---------|
| `docker` | Install Docker |
| `common` | Base configuration |
```

---

## Customizing Commands

### Modify Existing Commands

Edit files in `.claude/commands/` to change behavior:

**Example: Change default VLAN**

In `ip-find.md`, change:
```markdown
- `/ip-find next` - Next available in default VLAN (20)
```
To your default VLAN.

**Example: Add service category**

In `service-list.md`, add to categories:
```markdown
### Service Categories

1. **Media Stack** - ...
2. **Core Infrastructure** - ...
3. **Your New Category** - Description
```

### Create Custom Commands

Add new `.md` files to `.claude/commands/`:

```markdown
# My Custom Command

Description of what it does.

## Instructions

Step-by-step instructions for Claude...

## Data Sources

Read these files:
- `path/to/file1.md`
- `path/to/file2.md`

## Output Format

Template for output...

## Arguments

- `/my-command` - Default behavior
- `/my-command arg` - With argument
```

---

## Documentation Structure Options

### Option 1: Flat Structure

```
docs/
├── CLAUDE.md
├── network.md
├── services.md
├── ips.md
├── proxmox.md
└── troubleshooting.md
```

Update `CLAUDE.md`:
```markdown
## Documentation Structure

| File | Purpose |
|------|---------|
| `network.md` | Network docs |
| `services.md` | Services |
```

### Option 2: Folder Structure

```
docs/
├── CLAUDE.md
├── infrastructure/
│   ├── network.md
│   ├── proxmox.md
│   └── kubernetes.md
├── services/
│   ├── catalog.md
│   └── media-stack.md
└── operations/
    ├── ips.md
    └── troubleshooting.md
```

Update `CLAUDE.md`:
```markdown
## Documentation Structure

| Path | Purpose |
|------|---------|
| `infrastructure/network.md` | Network |
| `infrastructure/proxmox.md` | Proxmox |
| `services/catalog.md` | Service catalog |
| `operations/ips.md` | IP allocations |
```

### Option 3: Numbered Files (Wiki-style)

```
docs/
├── CLAUDE.md
├── 00 - Index.md
├── 01 - Network Architecture.md
├── 02 - Proxmox Cluster.md
├── 03 - Services.md
└── 10 - IP Address Map.md
```

Update command files to reference numbered paths.

---

## Customization Examples

### For Single-Node Setup

Simplify `CLAUDE.md`:
```markdown
## Infrastructure

- Single Proxmox host
- 10 Docker services
- No Kubernetes

## Documentation

| File | Purpose |
|------|---------|
| `services.md` | All services |
| `ips.md` | IP allocations |
```

### For Kubernetes-Focused Lab

Emphasize K8s in `CLAUDE.md`:
```markdown
## Primary Focus: Kubernetes

### Cluster Details
- 3 control plane nodes
- 5 worker nodes
- ArgoCD for GitOps
- Prometheus/Grafana monitoring

### Namespaces
| Namespace | Purpose |
|-----------|---------|
| media | Media applications |
| monitoring | Observability |
| default | General workloads |
```

### For Multi-Site Setup

Add site context:
```markdown
## Sites

### Site A - Primary
- Location: Home
- Proxmox cluster: 3 nodes
- VLAN: 10.0.10.0/24

### Site B - Remote
- Location: Colo
- Single server
- VLAN: 10.0.20.0/24

### Site Connectivity
- VPN: WireGuard
- Primary link: 192.168.100.0/24
```

---

## Tips

1. **Be specific** - The more detail in CLAUDE.md, the better
2. **Keep paths accurate** - Commands read files by path
3. **Use consistent naming** - Makes searching easier
4. **Document conventions** - Helps generate correct code
5. **Update regularly** - Keep CLAUDE.md current

---

## Troubleshooting

### Commands not finding files

- Check paths in CLAUDE.md
- Ensure files exist at specified locations
- Use relative paths from CLAUDE.md location

### Wrong output format

- Check command file for output template
- Modify template to match your preferences

### Missing features

- Add relevant sections to CLAUDE.md
- Create custom commands for specific needs
