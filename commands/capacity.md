# Capacity

Resource capacity planning and utilization analysis.

## Instructions

Analyze current resource utilization and plan for new deployments.

### Data Sources

Read these files:
- `07 HomeLab Things/Claude Managed Homelab/02 - Proxmox Cluster.md` - Compute resources
- `07 HomeLab Things/Claude Managed Homelab/04 - Kubernetes Cluster.md` - K8s resources
- `07 HomeLab Things/Claude Managed Homelab/03 - Storage Architecture.md` - Storage

### Resource Categories

1. **Compute** - CPU cores, RAM
2. **Storage** - Disk space, IOPS
3. **Network** - Bandwidth, IPs
4. **Kubernetes** - Pods, CPU/memory requests

### Output Format - Full Report

```markdown
# 📈 Capacity Planning Report

**Generated**: [Date]
**Cluster**: MorpheusCluster

---

## 🖥️ Compute Resources

### Per-Node Breakdown

| Node | vCPUs | vCPUs Used | RAM | RAM Used | VMs | Status |
|------|-------|------------|-----|----------|-----|--------|
| node01 | 24 | 16 (67%) | 64 GB | 45 GB (70%) | 8 | 🟢 OK |
| node02 | 20 | 14 (70%) | 48 GB | 32 GB (67%) | 6 | 🟢 OK |
| node03 | 24 | 14 (58%) | 32 GB | 24 GB (75%) | 4 | 🟡 RAM High |

### Cluster Totals

| Resource | Used | Total | Available | Utilization |
|----------|------|-------|-----------|-------------|
| vCPUs | 44 | 68 | 24 | 65% |
| RAM | 101 GB | 144 GB | 43 GB | 70% |
| VMs | 18 | ~50 | 32 | 36% |

### Utilization Graphs

```
CPU:     [████████████░░░░░░░░] 65%
Memory:  [██████████████░░░░░░] 70%
VM Slots:[███████░░░░░░░░░░░░░] 36%
```

---

## 💾 Storage

| Pool | Type | Used | Total | Available | Util% |
|------|------|------|-------|-----------|-------|
| VMDisks | NFS | 180 GB | 300 GB | 120 GB | 60% |
| Media | NFS | 2.1 TB | 4 TB | 1.9 TB | 52% |
| ISOs | NFS | 45 GB | 100 GB | 55 GB | 45% |
| local-lvm | LVM | 80 GB | 200 GB | 120 GB | 40% |
| **Total** | - | **2.4 TB** | **4.6 TB** | **2.2 TB** | **52%** |

### Storage Graphs

```
VMDisks: [████████████░░░░░░░░] 60%
Media:   [██████████░░░░░░░░░░] 52%
ISOs:    [█████████░░░░░░░░░░░] 45%
local:   [████████░░░░░░░░░░░░] 40%
```

---

## ☸️ Kubernetes Resources

| Resource | Requested | Allocatable | Available | Util% |
|----------|-----------|-------------|-----------|-------|
| CPU | 12 cores | 24 cores | 12 cores | 50% |
| Memory | 32 GB | 48 GB | 16 GB | 67% |
| Pods | 85 | 330 | 245 | 26% |

### Per-Node K8s

| Node | Role | CPU Req | Mem Req | Pods |
|------|------|---------|---------|------|
| k8s-cp-01 | Control | 2 | 4 GB | 15 |
| k8s-cp-02 | Control | 2 | 4 GB | 14 |
| k8s-cp-03 | Control | 2 | 4 GB | 13 |
| k8s-w-01 | Worker | 1 | 4 GB | 12 |
| ... | ... | ... | ... | ... |

---

## 🌐 Network (IPs)

| VLAN | Allocated | Total | Available | Util% |
|------|-----------|-------|-----------|-------|
| 20 - Homelab | 35 | 200 | 165 | 18% |
| 30 - IoT | 12 | 200 | 188 | 6% |
| 90 - Mgmt | 8 | 200 | 192 | 4% |

---

## 🎯 Recommendations

### Can Deploy Now

With current capacity, you can deploy:
- **[X] small VMs** (2 vCPU, 4GB RAM)
- **[X] medium VMs** (4 vCPU, 8GB RAM)
- **[X] large VMs** (8 vCPU, 16GB RAM)

### Optimal Placement

| VM Size | Recommended Node | Reason |
|---------|------------------|--------|
| Small | node03 | Lowest utilization |
| Medium | node03 | Best balance |
| Large | node01 | Most headroom |

### Alerts

⚠️ **node03 RAM**: 75% utilized - monitor closely
⚠️ **VMDisks**: 60% - plan for expansion if growth continues

### Upgrade Recommendations

| Priority | Upgrade | Benefit |
|----------|---------|---------|
| Medium | node03 RAM +32GB | Balance cluster |
| Low | Additional NVMe | Faster local storage |

---

## 📊 Growth Projection (3 months)

Based on current growth rate:

| Resource | Current | Projected | Status |
|----------|---------|-----------|--------|
| Storage | 52% | 65% | 🟢 OK |
| RAM | 70% | 78% | 🟡 Monitor |
| CPU | 65% | 70% | 🟢 OK |

---

## 🧮 Plan New VM

Use `/capacity plan [specs]` to check if a new VM fits.
```

### Output Format - Plan New VM

```markdown
# 🧮 Capacity Check: New VM

**Requested**: [X] vCPUs, [X] GB RAM, [X] GB disk

## Feasibility

✅ **CAN DEPLOY** / ❌ **INSUFFICIENT RESOURCES**

### Best Placement

| Node | After Deploy | Status |
|------|--------------|--------|
| node01 | CPU: 75%, RAM: 78% | 🟡 Possible |
| node02 | CPU: 80%, RAM: 75% | 🟡 Possible |
| node03 | CPU: 66%, RAM: 83% | 🟡 Possible |

**Recommendation**: Deploy to **node03**
- Lowest CPU utilization
- Acceptable RAM headroom

### Resource After Deployment

| Resource | Before | After | Change |
|----------|--------|-------|--------|
| Cluster CPU | 65% | 70% | +5% |
| Cluster RAM | 70% | 75% | +5% |
| node03 CPU | 58% | 66% | +8% |
| node03 RAM | 75% | 83% | +8% |

### Storage Check

| Pool | After | Status |
|------|-------|--------|
| VMDisks | 66% | 🟢 OK |

---

## ⚠️ Warnings

- node03 RAM will be at 83% after deployment
- Consider migrating a small VM from node03 first

## Next Steps

1. Run `/deploy-new [name]` to generate deployment code
2. Or adjust specs and re-check
```

## Arguments

- `/capacity` - Full capacity report
- `/capacity plan [cpu] [ram]` - Check if specs fit (e.g., `/capacity plan 4cpu 8gb`)
- `/capacity proxmox` - Proxmox only
- `/capacity k8s` - Kubernetes only
- `/capacity storage` - Storage only
- `/capacity network` - IP availability
- `/capacity forecast` - 3-month projection
- `/capacity node [name]` - Specific node details
