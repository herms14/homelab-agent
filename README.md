# 🏠 Homelab Agent

> **AI-powered homelab infrastructure management using Claude Code**

Transform your homelab management with intelligent automation. This skill pack gives Claude Code the ability to monitor, manage, document, and deploy infrastructure in your homelab environment.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-blue)](https://claude.ai/code)
[![Proxmox](https://img.shields.io/badge/Proxmox-Compatible-orange)](https://www.proxmox.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Compatible-326CE5)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-Compatible-2496ED)](https://docker.com/)

---

## ✨ Features

| Feature | Command | Description |
|---------|---------|-------------|
| 🏠 **Infrastructure Overview** | `/homelab` | Main dashboard with cluster status and alerts |
| 📊 **Status Reports** | `/lab-status` | Comprehensive infrastructure health reports |
| 📦 **Service Catalog** | `/service-list` | Complete service inventory with URLs |
| 🔍 **IP Management** | `/ip-find` | Find available IPs, prevent conflicts |
| 🚀 **Code Generator** | `/deploy-new` | Generate Terraform, Ansible, Docker code |
| 🔧 **Troubleshooting** | `/troubleshoot` | Search guides and diagnose issues |
| 📋 **Change Tracking** | `/lab-changelog` | Infrastructure audit trail |
| 📖 **Runbook Generator** | `/runbook` | Operational procedure documentation |
| 📈 **Capacity Planning** | `/capacity` | Resource utilization and planning |
| 🔄 **Doc Verification** | `/doc-sync` | Keep documentation accurate |

---

## 🎯 Who Is This For?

This agent is designed for homelab enthusiasts running:

- **Proxmox VE** clusters (single node or multi-node)
- **Kubernetes** clusters (kubeadm, k3s, RKE, etc.)
- **Docker** hosts with multiple services
- **Infrastructure as Code** (Terraform, Ansible)
- **Comprehensive documentation** in Obsidian or similar

### Perfect For:

- 🏗️ Managing complex multi-node clusters
- 📝 Keeping documentation in sync with reality
- 🔍 Finding available IPs across VLANs
- 🚀 Quickly deploying new services
- 🔧 Troubleshooting infrastructure issues
- 📈 Planning capacity for new workloads

---

## 🚀 Quick Start

### Prerequisites

- [Claude Code](https://claude.ai/code) installed
- Homelab documentation in markdown format
- Basic command line familiarity

### Installation

1. **Clone this repository** into your documentation folder:

   ```bash
   # Navigate to your documentation root
   cd "path/to/your/docs"

   # Clone into .claude folder
   git clone https://github.com/herms14/homelab-agent .claude
   ```

   Or **copy manually**:
   - Copy the `commands/` folder to `.claude/commands/`
   - Copy `CLAUDE.md` to your docs root

2. **Customize CLAUDE.md** for your environment (see [Customization](#-customization))

3. **Start Claude Code**:

   ```bash
   cd "path/to/your/docs"
   claude
   ```

4. **Run your first command**:

   ```
   /homelab
   ```

---

## 📖 Available Commands

### Core Commands

| Command | Description |
|---------|-------------|
| `/homelab` | 🏠 Main menu showing infrastructure summary and available actions |
| `/lab-status` | 📊 Comprehensive status report for all infrastructure components |
| `/service-list` | 📦 List all deployed services with URLs, ports, and auth methods |

### Infrastructure Management

| Command | Description |
|---------|-------------|
| `/ip-find` | 🔍 Find available IPs, check allocations, reserve addresses |
| `/deploy-new` | 🚀 Generate deployment code (Terraform, Docker, Ansible) |
| `/capacity` | 📈 Resource utilization analysis and capacity planning |

### Operations & Documentation

| Command | Description |
|---------|-------------|
| `/troubleshoot` | 🔧 Search troubleshooting guides and get diagnostic commands |
| `/lab-changelog` | 📋 Log infrastructure changes for audit trail |
| `/runbook` | 📖 Generate operational procedures and runbooks |
| `/doc-sync` | 🔄 Verify documentation matches actual infrastructure |

---

## 🏠 Main Dashboard

```
╭──────────────────────────────────────────────────────────────────╮
│  🏠 Homelab Agent - MorpheusCluster                              │
╰──────────────────────────────────────────────────────────────────╯

Infrastructure Summary:
  • Proxmox Nodes: 3 (Healthy)
  • VMs Running: 18
  • K8s Nodes: 9
  • Docker Services: 33+

┌─────────────────────────────────────────────────────────────────┐
│  📋 Quick Actions                                               │
├──────────────────┬──────────────────────────────────────────────┤
│  /lab-status     │  📊 Full infrastructure status               │
│  /service-list   │  📦 List all services with URLs              │
│  /ip-find        │  🔍 Find available IP addresses              │
│  /deploy-new     │  🚀 Generate deployment code                 │
│  /troubleshoot   │  🔧 Search troubleshooting guides            │
└──────────────────┴──────────────────────────────────────────────┘

⚠️  Alerts:
  • 2 services pending Watchtower update
  • PBS backup completed 2 hours ago
```

---

## 📊 Infrastructure Status Report

The `/lab-status` command generates comprehensive reports:

```markdown
# 📊 Homelab Status Report

## 🖥️ Proxmox Cluster

| Node | Status | CPU | RAM | VMs |
|------|--------|-----|-----|-----|
| node01 | 🟢 Online | 23% | 45/64 GB | 8 |
| node02 | 🟢 Online | 18% | 32/48 GB | 6 |
| node03 | 🟢 Online | 12% | 24/32 GB | 4 |

## ☸️ Kubernetes Cluster

| Component | Count | Status |
|-----------|-------|--------|
| Control Plane | 3 | 🟢 Healthy |
| Workers | 6 | 🟢 Ready |

## 📦 Services

| Category | Running | Status |
|----------|---------|--------|
| Media Stack | 14/14 | 🟢 All Running |
| Core Services | 8/8 | 🟢 All Running |
| Monitoring | 5/5 | 🟢 All Running |
```

---

## 🔍 IP Address Management

Never have IP conflicts again! The `/ip-find` command:

```markdown
# 🔍 IP Address Finder

## VLAN 20 - Homelab (192.168.20.0/24)

### Allocated (35 IPs)
| IP | Device | Type |
|----|--------|------|
| 192.168.20.20 | node01 | Proxmox |
| 192.168.20.21 | node02 | Proxmox |
| 192.168.20.50 | k8s-cp-01 | K8s Control |
| ... | ... | ... |

### Next Available
✅ **192.168.20.100** ← Recommended for new VM
```

### Arguments

- `/ip-find` - Summary of all VLANs
- `/ip-find vlan20` - Specific VLAN
- `/ip-find check 192.168.20.100` - Check if IP is in use
- `/ip-find next` - Next available IP

---

## 🚀 Deployment Code Generator

Generate infrastructure code instantly with `/deploy-new`:

```bash
/deploy-new docker jellyfin
```

Generates:

```yaml
# docker-compose.jellyfin.yml
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - "8096:8096"
    volumes:
      - /srv/jellyfin/config:/config
      - /srv/jellyfin/media:/media
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.jellyfin.rule=Host(`jellyfin.domain.xyz`)"
```

Plus:
- Terraform VM configuration
- Traefik routing rules
- Authentik SSO setup
- Documentation updates

---

## ⚙️ Customization

### Configure CLAUDE.md

The agent reads `CLAUDE.md` from your documentation root for context. Customize it for your environment:

```markdown
## Homelab Overview

**Cluster Name**: MorpheusCluster
**Domain**: yourdomain.xyz
**Primary VLAN**: 20 (192.168.20.0/24)

## Infrastructure

### Proxmox Nodes
| Node | IP | Role |
|------|-----|------|
| node01 | 192.168.20.20 | Primary |
| node02 | 192.168.20.21 | Secondary |

### Documentation Files
| File | Purpose |
|------|---------|
| `network.md` | Network architecture |
| `services.md` | Service catalog |
| `ips.md` | IP allocations |
```

### Documentation Structure

The agent works best with organized documentation:

```
your-docs/
├── .claude/
│   └── commands/        # Agent skills
├── CLAUDE.md            # Agent context
├── network.md           # Network architecture
├── proxmox.md           # Cluster documentation
├── kubernetes.md        # K8s documentation
├── services.md          # Service catalog
├── ips.md               # IP allocations
├── troubleshooting.md   # Known issues
└── changelog.md         # Change log
```

---

## 📁 File Structure

```
homelab-agent/
├── commands/
│   ├── homelab.md           # Main menu
│   ├── lab-status.md        # Status reports
│   ├── service-list.md      # Service catalog
│   ├── ip-find.md           # IP management
│   ├── deploy-new.md        # Code generator
│   ├── troubleshoot.md      # Troubleshooting
│   ├── lab-changelog.md     # Change logging
│   ├── runbook.md           # Runbook generator
│   ├── capacity.md          # Capacity planning
│   └── doc-sync.md          # Doc verification
├── docs/
│   ├── INSTALLATION.md      # Setup guide
│   ├── COMMANDS.md          # Command reference
│   └── CUSTOMIZATION.md     # Customization guide
├── CLAUDE.md.template       # Template for users
├── LICENSE                  # MIT License
└── README.md                # This file
```

---

## 🎨 Use Cases

### Daily Operations

```bash
# Morning status check
/lab-status quick

# Check service URLs
/service-list

# Review any changes
/lab-changelog today
```

### Deploying New Services

```bash
# Check if resources available
/capacity plan 4cpu 8gb

# Find an IP
/ip-find next vlan20

# Generate deployment code
/deploy-new docker my-service

# Log the change
/lab-changelog add "Deployed my-service"
```

### Troubleshooting

```bash
# Search for known issues
/troubleshoot container won't start

# Generate diagnostic commands
/troubleshoot diag traefik

# Check documentation for related info
/doc-sync services
```

### Maintenance

```bash
# Generate maintenance runbook
/runbook maintenance

# Plan node upgrade
/capacity node node01

# Verify docs are current
/doc-sync
```

---

## 🔗 Integration

### Works With

| Tool | Integration |
|------|-------------|
| **Proxmox VE** | Reads cluster/VM documentation |
| **Kubernetes** | Understands K8s concepts |
| **Docker** | Generates compose files |
| **Terraform** | Generates HCL modules |
| **Ansible** | Generates playbooks |
| **Traefik** | Routing configuration |
| **Authentik** | SSO setup guidance |

### Pairs Well With

- [Obsidian Vault Agent](https://github.com/herms14/obsidian-vault-agent) - General vault maintenance
- Your existing IaC workflows
- GitOps pipelines

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Ideas for Contributions

- [ ] Additional deployment templates
- [ ] More runbook templates
- [ ] Integration with monitoring APIs
- [ ] Cost tracking features

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- The homelab community for inspiration
- [Proxmox](https://www.proxmox.com/) for amazing virtualization
- [Claude Code](https://claude.ai/code) for AI-powered automation
- [Obsidian](https://obsidian.md) for knowledge management

---

## 📬 Support

- 🐛 **Issues**: [GitHub Issues](https://github.com/herms14/homelab-agent/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/herms14/homelab-agent/discussions)
- 🏠 **Reddit**: r/homelab, r/selfhosted

---

<p align="center">
  Made with ❤️ for the homelab community
</p>
