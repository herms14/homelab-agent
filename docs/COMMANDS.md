# Command Reference

> Complete reference for all Homelab Agent commands

---

## Command Overview

| Command | Category | Purpose |
|---------|----------|---------|
| `/homelab` | Core | Main menu and dashboard |
| `/lab-status` | Core | Infrastructure status reports |
| `/service-list` | Core | Service catalog |
| `/ip-find` | Management | IP address management |
| `/deploy-new` | Management | Deployment code generator |
| `/capacity` | Management | Resource capacity planning |
| `/troubleshoot` | Operations | Troubleshooting assistant |
| `/lab-changelog` | Operations | Change tracking |
| `/runbook` | Operations | Runbook generator |
| `/doc-sync` | Operations | Documentation verification |

---

## Core Commands

### /homelab

**Main menu and infrastructure dashboard**

Displays infrastructure summary, available commands, and current alerts.

```bash
/homelab              # Show main menu
/homelab quick        # Just show commands
/homelab alerts       # Focus on alerts only
```

**Output includes:**
- Infrastructure summary (nodes, VMs, services)
- Quick action menu
- Current alerts and warnings

---

### /lab-status

**Comprehensive infrastructure status report**

Generates detailed status for all infrastructure components.

```bash
/lab-status           # Full report
/lab-status proxmox   # Proxmox cluster only
/lab-status k8s       # Kubernetes only
/lab-status services  # Services only
/lab-status storage   # Storage only
/lab-status network   # Network only
/lab-status quick     # Summary only
```

**Report sections:**
- Proxmox cluster health
- Kubernetes cluster status
- Service status by category
- Storage utilization
- Network overview
- Alerts and recommendations

---

### /service-list

**Service catalog with URLs and details**

Lists all deployed services with access information.

```bash
/service-list                    # All services
/service-list media              # Media stack only
/service-list core               # Core infrastructure
/service-list monitoring         # Monitoring services
/service-list utilities          # Utility services
/service-list search [term]      # Search by name
/service-list url [service]      # Get URL for service
/service-list port [number]      # Find service by port
```

**Information provided:**
- Service name and description
- Access URL (internal/external)
- Backend type (Docker/LXC/K8s)
- Port number
- Authentication method

---

## Management Commands

### /ip-find

**IP address management and allocation**

Find available IPs, check allocations, and prevent conflicts.

```bash
/ip-find                              # All VLANs summary
/ip-find vlan20                       # Specific VLAN
/ip-find homelab                      # By VLAN name
/ip-find check 192.168.20.100         # Check if IP is used
/ip-find next                         # Next available (default VLAN)
/ip-find next vlan30                  # Next available in VLAN
/ip-find reserve 192.168.20.100 "VM"  # Reserve and document
/ip-find search [term]                # Search by device name
```

**Features:**
- Shows allocated vs available IPs
- Displays reserved ranges
- Suggests next available IP
- Detects potential conflicts
- Updates documentation

---

### /deploy-new

**Deployment code generator**

Generate Terraform, Docker Compose, and Ansible code for new services.

```bash
/deploy-new                           # Interactive wizard
/deploy-new docker [name]             # Docker service only
/deploy-new vm [name]                 # New VM with service
/deploy-new lxc [name]                # LXC container
/deploy-new k8s [name]                # Kubernetes deployment
/deploy-new [name] --image [img:tag]  # Specify Docker image
/deploy-new [name] --port [port]      # Specify port
```

**Generates:**
- Terraform VM/LXC definition
- Docker Compose file
- Traefik routing configuration
- Authentik SSO setup
- Documentation updates
- Deployment checklist

---

### /capacity

**Resource capacity planning**

Analyze current utilization and plan new deployments.

```bash
/capacity                    # Full capacity report
/capacity plan 4cpu 8gb      # Check if specs fit
/capacity proxmox            # Proxmox only
/capacity k8s                # Kubernetes only
/capacity storage            # Storage only
/capacity network            # IP availability
/capacity forecast           # 3-month projection
/capacity node [name]        # Specific node details
```

**Analysis includes:**
- Per-node resource breakdown
- Cluster totals and utilization
- Storage pool status
- Kubernetes resource requests
- Placement recommendations
- Growth projections

---

## Operations Commands

### /troubleshoot

**Troubleshooting assistant**

Search known issues and get diagnostic guidance.

```bash
/troubleshoot [description]     # Search for issue
/troubleshoot docker            # Docker-specific help
/troubleshoot network           # Network issues
/troubleshoot proxmox           # Proxmox issues
/troubleshoot k8s               # Kubernetes issues
/troubleshoot auth              # Authentication issues
/troubleshoot [service-name]    # Service-specific help
/troubleshoot add               # Add new troubleshooting entry
/troubleshoot diag [service]    # Generate diagnostic commands
```

**Provides:**
- Matching known issues
- Step-by-step solutions
- Diagnostic commands
- Related documentation links
- Option to add new entries

---

### /lab-changelog

**Infrastructure change tracking**

Maintain audit trail of all infrastructure changes.

```bash
/lab-changelog                    # View recent entries
/lab-changelog add "Description"  # Add new entry
/lab-changelog add                # Interactive add
/lab-changelog today              # Today's changes
/lab-changelog week               # This week's changes
/lab-changelog month              # This month's changes
/lab-changelog search [term]      # Search changelog
/lab-changelog stats              # Show statistics
```

**Change categories:**
- Added - New resources
- Changed - Modifications
- Fixed - Bug fixes
- Removed - Decommissioned items
- Infrastructure - Hardware changes
- Security - Security updates

---

### /runbook

**Operational runbook generator**

Generate step-by-step procedures for common tasks.

```bash
/runbook                         # List available runbooks
/runbook maintenance             # Node maintenance procedure
/runbook backup                  # Backup procedures
/runbook disaster-recovery       # DR procedures (or /runbook dr)
/runbook service-restart [svc]   # Restart specific service
/runbook upgrade [component]     # Upgrade procedure
/runbook scale [component]       # Scaling procedure
/runbook network [change]        # Network changes
/runbook security [task]         # Security procedures
/runbook custom [title]          # Generate custom runbook
```

**Runbook sections:**
- Overview and metadata
- Prerequisites
- Pre-checks
- Step-by-step procedure
- Verification steps
- Rollback procedure
- Troubleshooting

---

### /doc-sync

**Documentation verification**

Verify documentation accuracy and find discrepancies.

```bash
/doc-sync                # Full sync report
/doc-sync services       # Service catalog only
/doc-sync ips            # IP addresses only
/doc-sync configs        # Configurations only
/doc-sync stale          # Stale content only
/doc-sync fix            # Generate fix suggestions
/doc-sync apply          # Apply auto-fixes
/doc-sync [filename]     # Check specific file
```

**Checks performed:**
- Missing documentation entries
- Incorrect information
- Outdated entries
- Potential duplicates
- Stale content detection

---

## Command Cheat Sheet

### Daily Operations

```bash
/lab-status quick        # Morning status check
/service-list            # Quick service lookup
/lab-changelog today     # Review changes
```

### Deploying Services

```bash
/capacity plan 4cpu 8gb  # Check resources
/ip-find next vlan20     # Find IP
/deploy-new docker svc   # Generate code
/lab-changelog add "..."  # Log change
```

### Troubleshooting

```bash
/troubleshoot [issue]    # Search issues
/troubleshoot diag svc   # Get diagnostics
/doc-sync services       # Verify docs
```

### Maintenance

```bash
/runbook maintenance     # Get procedure
/capacity node node01    # Check resources
/doc-sync                # Verify accuracy
```

---

## Tips & Best Practices

1. **Start with `/homelab`** to see the overview
2. **Use `/lab-status quick`** for fast daily checks
3. **Always log changes** with `/lab-changelog`
4. **Run `/doc-sync`** periodically to keep docs current
5. **Use `/capacity plan`** before deploying new VMs
6. **Generate runbooks** for repeatable procedures

---

## Getting Help

If a command isn't working as expected:

1. Check that `CLAUDE.md` has correct file paths
2. Ensure documentation files exist
3. Try running with more specific arguments
4. Check [GitHub Issues](https://github.com/herms14/homelab-agent/issues)
