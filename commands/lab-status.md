# Lab Status

Comprehensive infrastructure status report for the homelab.

## Instructions

Generate a detailed status report covering all infrastructure components.

### Data Sources

Read `CLAUDE.md` first. Then, using its **Documentation Structure** table, read these docs by role:
- **Proxmox** - Nodes, VMs, LXCs
- **Kubernetes** - K8s info (skip if missing or listed under Retired Components)
- **Services** - All services
- **Storage** - Pools, NAS, backups
- **Network** - VLANs, DNS, remote access
- **Monitoring** - Monitoring stack
- **Changelog** - Recent activity
- **Task Registry** - Open work

Never open the doc listed under the **Sensitive** role.

### Live Data (optional)

If the user has shell access configured (SSH to a Proxmox node), prefer live data over docs and say which you used:

```bash
pvecm status                                  # Quorum
pvesh get /cluster/resources --type node      # Node CPU / RAM
pvesh get /cluster/resources --type vm        # VMs + LXCs with status
pvesm status                                  # Storage pools
```

Only run read-only commands. Note any difference between live data and docs and suggest `/doc-sync`.

### Output Format

```markdown
# 📊 Homelab Status Report

**Generated**: [Current Date/Time]
**Cluster**: [Cluster Name]
**Source**: Documentation / Live (pvesh) / Mixed

---

## 🖥️ Proxmox Cluster

| Node | Role | IP | CPU | RAM | VMs | LXCs |
|------|------|-----|-----|-----|-----|------|
| [node01] | Primary | [ip] | [X] cores | [X] GB | [X] | [X] |
| [node02] | Secondary | [ip] | [X] cores | [X] GB | [X] | [X] |

**Cluster Health**: ✅ Quorum OK / ⚠️ Degraded / 🔴 Critical
**Total Resources**: [X] vCPUs, [X] GB RAM
**Version**: Proxmox VE [X]

---

## 📦 Guests

| ID | Name | Type | Node | Status | Purpose |
|----|------|------|------|--------|---------|
| [100] | [name] | LXC | [node] | 🟢 Running | [purpose] |
| [101] | [name] | VM | [node] | 🔴 Stopped | [purpose] |

Templates are listed separately and not counted as running guests.

---

## ☸️ Kubernetes Cluster

*(Omit this section if Kubernetes is not used or is retired.)*

| Component | Count | Status |
|-----------|-------|--------|
| Control Plane | [X] | 🟢 Healthy |
| Workers | [X] | 🟢 Ready |

---

## 🧩 Services by Category

| Category | Count | Status |
|----------|-------|--------|
| Media | [X] | 🟢 All Running |
| Core Infrastructure | [X] | 🟢 All Running |
| Monitoring | [X] | 🟢 All Running |
| Utilities | [X] | 🟢 All Running |
| **Total** | **[X]+** | |

---

## 💾 Storage & Backups

| Pool | Type | Used | Total | Utilization |
|------|------|------|-------|-------------|
| [pool] | NFS | [X] GB | [X] GB | [X]% |
| local-lvm | LVM | [X] GB | [X] GB | [X]% |

**Last backup**: [date / unknown]

---

## 🌐 Network

| VLAN | Name | Subnet | Devices |
|------|------|--------|---------|
| [20] | [Servers] | [subnet] | [X] |

**DNS**: [server] · **Remote access**: [Tailscale/WireGuard/None]

---

## 📈 Resource Utilization

```
CPU:     [████████░░░░░░░░] 50%
Memory:  [██████████░░░░░░] 70%
Storage: [████████░░░░░░░░] 55%
```

---

## 🗂️ Open Tasks

| Status | Task | Since |
|--------|------|-------|
| 🔄 In Progress | [task] | [date] |
| ⏸️ Blocked | [task] | [date] |

---

## ⚠️ Alerts & Warnings

- [Capacity warnings]
- [Stopped guests that should be running]
- [Relevant Known Gotchas from CLAUDE.md]

---

## 📋 Recent Activity

- [Last 3-5 Changelog entries]

---

## 🎯 Recommendations

1. [Resource optimization suggestions]
2. [Maintenance recommendations]
3. [Upgrade suggestions]
```

### Status Indicators

- 🟢 Healthy/Running/OK
- 🟡 Warning/Degraded
- 🔴 Critical/Down
- ⚪ Unknown/N/A

## Arguments

- `/lab-status` - Full comprehensive report
- `/lab-status proxmox` - Proxmox cluster only
- `/lab-status guests` - VM / LXC inventory only
- `/lab-status k8s` - Kubernetes only
- `/lab-status services` - Services only
- `/lab-status storage` - Storage and backups only
- `/lab-status network` - Network only
- `/lab-status tasks` - Open tasks only
- `/lab-status quick` - Summary dashboard only
- `/lab-status live` - Force live data via read-only Proxmox commands
