# IP Find

Find available IP addresses and manage IP allocations across VLANs.

## Instructions

Search and manage IP address allocations in the homelab network.

### Data Sources

Read `CLAUDE.md` first: VLANs, default VLAN, DNS server, IP Allocation Conventions, **Retired Components**, and **Known Gotchas**. Then, using its **Documentation Structure** table, read:
- **IP Map** - IP allocations
- **Network** - VLAN details
- **Proxmox** - Guest IPs that may be missing from the IP Map

### Rules

- An IP is **in use** if it appears in the IP Map, the Proxmox doc, the Services doc, or the Known Gotchas list (e.g. a dead NIC that must never be reused).
- IPs belonging to **Retired Components** are free unless re-allocated since.
- If two docs disagree, or the same IP is assigned twice, flag it as a ⚠️ conflict.
- If the user can run commands, suggest a live check before using an IP: `ping -c 2 [ip]` and `arping -c 2 [ip]` (or `ip neigh | grep [ip]` on a Proxmox node).

### Output Format - Find Available

```markdown
# 🔍 IP Address Finder

## VLAN [ID] - [Name] ([Subnet])

### Currently Allocated

| IP Address | Device/Service | Type | Notes |
|------------|----------------|------|-------|
| 10.0.20.1 | Gateway | Network | |
| 10.0.20.11 | node01 | Proxmox | Primary node |
| 10.0.20.12 | node02 | Proxmox | |
| 10.0.20.30 | NAS (eth0) | Storage | ⚠️ Do not use (Known Gotcha) |
| ... | ... | ... | ... |

### Reserved Ranges (from CLAUDE.md conventions)

| Range | Purpose |
|-------|---------|
| .1-.10 | Network infrastructure |
| .20-.30 | Physical servers |
| .200-.254 | DHCP pool |

### Freed by Retired Components

| Range | Previously |
|-------|-----------|
| [.50-.70] | [Kubernetes cluster, retired YYYY-MM-DD] |

### Next Available IPs

✅ **Recommended**: [subnet].[X]

Available in sequence:
1. [subnet].[X]
2. [subnet].[Y]
3. [subnet].[Z]

### Usage Summary

- **Total IPs**: 254
- **Allocated**: [X]
- **Reserved**: [X]
- **Available**: [X]
- **Utilization**: [X]%
```

### Output Format - Check IP

```markdown
# 🔍 IP Check: [ip]

**Status**: ✅ Available / 🔴 In Use / ⚠️ Conflict

## If In Use:
| Property | Value |
|----------|-------|
| Device | [Name] |
| Type | [VM/LXC/Physical] |
| Purpose | [Description] |
| Source | [Which doc(s)] |

## If Available:
This IP is available for use.

**VLAN**: [ID] - [Name]
**Subnet**: [subnet]
**Gateway**: [gateway]
**DNS**: [DNS server from CLAUDE.md]
```

### Output Format - All VLANs Summary

```markdown
# 🌐 IP Address Summary - All VLANs

| VLAN | Name | Subnet | Allocated | Available | Util% |
|------|------|--------|-----------|-----------|-------|
| [10] | [Management] | [subnet] | [X] | [Y] | [Z]% |
| [20] | [Servers] | [subnet] | [X] | [Y] | [Z]% |
| ... | ... | ... | ... | ... | ... |

## ⚠️ Conflicts

| IP | Claimed By |
|----|------------|
| [ip] | [device A], [device B] |

## Recommendations

- VLAN with most space: [Name]
- Suggested for new VMs: [default VLAN]
```

### Reserving an IP

When reserving, follow the Change Documentation Protocol in `CLAUDE.md`:
1. Add the row to the **IP Map** doc:
   ```markdown
   | [ip] | [New Service] | [Type] | [Notes] |
   ```
2. Add a Changelog entry (`Added` category)
3. Update `updated:` frontmatter on modified docs

Ask for confirmation before writing.

## Arguments

- `/ip-find` - Show all VLANs summary
- `/ip-find vlan20` or `/ip-find [vlan-name]` - Specific VLAN
- `/ip-find check [ip]` - Check if IP is in use
- `/ip-find next` - Next available in the default VLAN from CLAUDE.md
- `/ip-find next vlan30` - Next available in specific VLAN
- `/ip-find reserve [ip] "Service Name"` - Reserve and document
- `/ip-find search [term]` - Search allocations by name
- `/ip-find conflicts` - Only show duplicate / conflicting allocations
