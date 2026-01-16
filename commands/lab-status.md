# Lab Status

Comprehensive infrastructure status report for the homelab.

## Instructions

Generate a detailed status report covering all infrastructure components.

### Data Sources

Read these files:
- `07 HomeLab Things/Claude Managed Homelab/02 - Proxmox Cluster.md` - Cluster details
- `07 HomeLab Things/Claude Managed Homelab/04 - Kubernetes Cluster.md` - K8s info
- `07 HomeLab Things/Claude Managed Homelab/07 - Deployed Services.md` - All services
- `07 HomeLab Things/Claude Managed Homelab/03 - Storage Architecture.md` - Storage
- `07 HomeLab Things/Claude Managed Homelab/01 - Network Architecture.md` - Network
- `07 HomeLab Things/Claude Managed Homelab/17 - Monitoring Stack.md` - Monitoring

### Output Format

```markdown
# 📊 Homelab Status Report

**Generated**: [Current Date/Time]
**Cluster**: MorpheusCluster

---

## 🖥️ Proxmox Cluster

| Node | Role | IP | CPU | RAM | VMs | LXCs |
|------|------|-----|-----|-----|-----|------|
| node01 | Primary | 192.168.20.20 | [X] cores | [X] GB | [X] | [X] |
| node02 | Secondary | 192.168.20.21 | [X] cores | [X] GB | [X] | [X] |
| node03 | Tertiary | 192.168.20.22 | [X] cores | [X] GB | [X] | [X] |

**Cluster Health**: ✅ Quorum OK / ⚠️ Degraded / 🔴 Critical
**Total Resources**: [X] vCPUs, [X] GB RAM

---

## ☸️ Kubernetes Cluster

| Component | Count | Status |
|-----------|-------|--------|
| Control Plane | 3 | 🟢 Healthy |
| Workers | 6 | 🟢 Ready |
| Total Nodes | 9 | [Status] |

**Version**: v1.28.15
**CNI**: Calico v3.27.0
**Runtime**: containerd v1.7.28

---

## 📦 Services by Category

| Category | Count | Status |
|----------|-------|--------|
| Media Stack | [X] | 🟢 All Running |
| Core Infrastructure | [X] | 🟢 All Running |
| Monitoring | [X] | 🟢 All Running |
| Utilities | [X] | 🟢 All Running |
| **Total** | **[X]+** | |

---

## 💾 Storage

| Pool | Type | Used | Total | Utilization |
|------|------|------|-------|-------------|
| VMDisks | NFS | [X] GB | [X] GB | [X]% |
| Media | NFS | [X] TB | [X] TB | [X]% |
| ISOs | NFS | [X] GB | [X] GB | [X]% |
| local-lvm | LVM | [X] GB | [X] GB | [X]% |

---

## 🌐 Network

| VLAN | Name | Subnet | Devices |
|------|------|--------|---------|
| 1 | Default | 192.168.0.0/24 | [X] |
| 10 | Internal | 192.168.10.0/24 | [X] |
| 20 | Homelab | 192.168.20.0/24 | [X] |
| 30 | IoT | 192.168.30.0/24 | [X] |
| ... | ... | ... | ... |

---

## 📈 Resource Utilization

```
CPU:     [████████░░░░░░░░] 50%
Memory:  [██████████░░░░░░] 70%
Storage: [████████░░░░░░░░] 55%
```

---

## ⚠️ Alerts & Warnings

- [List any capacity warnings]
- [List any service issues]
- [List any recent failures]

---

## 📋 Recent Activity

- [Recent VM changes]
- [Recent service deployments]
- [Recent config updates]

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
- `/lab-status k8s` - Kubernetes only
- `/lab-status services` - Services only
- `/lab-status storage` - Storage only
- `/lab-status network` - Network only
- `/lab-status quick` - Summary dashboard only
