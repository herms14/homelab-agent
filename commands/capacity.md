# Capacity

Resource capacity planning and utilization analysis.

## Instructions

Analyze current resource utilization and plan for new deployments.

### Data Sources

Read `CLAUDE.md` first (node specs, storage pools, **Retired Components**). Then, using its **Documentation Structure** table, read:
- **Proxmox** - Compute resources and guest allocations
- **Storage** - Pools, NAS, backups
- **IP Map** - IP availability
- **Kubernetes** - K8s resources (skip if missing or retired)

### Live Data (optional)

If the user has shell access to a Proxmox node, prefer live numbers and say so:

```bash
pvesh get /cluster/resources --type node --output-format json
pvesh get /cluster/resources --type vm --output-format json
pvesm status
```

Read-only commands only.

### Resource Categories

1. **Compute** - CPU cores, RAM (allocated vs physical; note overcommit)
2. **Storage** - Disk space per pool
3. **Network** - Free IPs per VLAN
4. **Kubernetes** - Pods, CPU/memory requests (only if active)

### Output Format - Full Report

````markdown
# 📈 Capacity Planning Report

**Generated**: [Date]
**Cluster**: [Cluster Name]
**Source**: Documentation / Live / Mixed

---

## 🖥️ Compute Resources

### Per-Node Breakdown

| Node | Cores | RAM | RAM Allocated | Guests | Status |
|------|-------|-----|---------------|--------|--------|
| [node01] | [16] | [64 GB] | [45 GB (70%)] | [10] | 🟢 OK |
| [node02] | [16] | [32 GB] | [28 GB (88%)] | [4] | 🟡 RAM High |

### Cluster Totals

| Resource | Allocated | Total | Available | Utilization |
|----------|-----------|-------|-----------|-------------|
| Cores | [X] | [X] | [X] | [X]% |
| RAM | [X] GB | [X] GB | [X] GB | [X]% |

```
CPU:     [████████████░░░░░░░░] 65%
Memory:  [██████████████░░░░░░] 70%
```

### Largest Consumers

| Guest | Node | RAM | Notes |
|-------|------|-----|-------|
| [name] | [node] | [X] GB | [right-size?] |

---

## 💾 Storage

| Pool | Type | Used | Total | Available | Util% |
|------|------|------|-------|-----------|-------|
| [pool] | NFS | [X] | [X] | [X] | [X]% |
| local-lvm | LVM | [X] | [X] | [X] | [X]% |

---

## ☸️ Kubernetes Resources

*(Omit if Kubernetes is not used or is retired.)*

---

## 🌐 Network (IPs)

| VLAN | Allocated | Usable | Available | Util% |
|------|-----------|--------|-----------|-------|
| [20] | [X] | [X] | [X] | [X]% |

---

## 🎯 Recommendations

### Can Deploy Now
- **[X] small guests** (2 vCPU, 2 GB RAM)
- **[X] medium guests** (4 vCPU, 8 GB RAM)
- **[X] large guests** (8 vCPU, 16 GB RAM)

### Optimal Placement

| Size | Recommended Node | Reason |
|------|------------------|--------|
| Small | [node] | Lowest utilization |
| Large | [node] | Most free RAM |

### Alerts
⚠️ [Node or pool above 80%]

### Upgrade / Right-Sizing Suggestions

| Priority | Action | Benefit |
|----------|--------|---------|
| [Medium] | [Add RAM to node02] | [Balance cluster] |
| [Low] | [Shrink an over-provisioned VM] | [Free RAM] |
````

### Output Format - Plan New Guest

```markdown
# 🧮 Capacity Check: New Guest

**Requested**: [X] vCPUs, [X] GB RAM, [X] GB disk

## Feasibility

✅ **CAN DEPLOY** / ❌ **INSUFFICIENT RESOURCES**

### Best Placement

| Node | RAM After | Status |
|------|-----------|--------|
| [node01] | [78%] | 🟡 Possible |
| [node02] | [95%] | 🔴 Avoid |

**Recommendation**: Deploy to **[node]**
- [Reason]

**LXC or VM?** Suggest an LXC when the workload does not need its own kernel; it uses far less RAM.

## Next Steps

1. `/ip-find next` to pick an IP
2. `/deploy-new [name]` to generate deployment code
```

### Thresholds

- 🟢 < 70% · 🟡 70-85% · 🔴 > 85%
- Keep enough free RAM on the cluster to absorb the largest node's guests if you rely on HA or migration.

## Arguments

- `/capacity` - Full capacity report
- `/capacity plan [cpu] [ram]` - Check if specs fit (e.g., `/capacity plan 4cpu 8gb`)
- `/capacity proxmox` - Proxmox only
- `/capacity k8s` - Kubernetes only
- `/capacity storage` - Storage only
- `/capacity network` - IP availability
- `/capacity forecast` - 3-month projection based on Changelog growth
- `/capacity node [name]` - Specific node details
