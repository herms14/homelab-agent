# IP Find

Find available IP addresses and manage IP allocations across VLANs.

## Instructions

Search and manage IP address allocations in the homelab network.

### Data Sources

Read these files:
- `07 HomeLab Things/Claude Managed Homelab/10 - IP Address Map.md` - IP allocations
- `07 HomeLab Things/Claude Managed Homelab/01 - Network Architecture.md` - VLAN info

### VLAN Reference

| VLAN ID | Name | Subnet | Purpose |
|---------|------|--------|---------|
| 1 | Default | 192.168.0.0/24 | General network |
| 10 | Internal | 192.168.10.0/24 | Internal services |
| 20 | Homelab | 192.168.20.0/24 | Lab infrastructure |
| 30 | IoT | 192.168.30.0/24 | IoT devices |
| 40 | Production | 192.168.40.0/24 | Production services |
| 50 | Guest | 192.168.50.0/24 | Guest network |
| 60 | Sonos | 192.168.60.0/24 | Audio devices |
| 90 | Management | 192.168.90.0/24 | Network management |

### Output Format - Find Available

```markdown
# 🔍 IP Address Finder

## VLAN [ID] - [Name] ([Subnet])

### Currently Allocated

| IP Address | Device/Service | Type | Notes |
|------------|----------------|------|-------|
| 192.168.20.20 | node01 | Proxmox | Primary node |
| 192.168.20.21 | node02 | Proxmox | Secondary |
| 192.168.20.22 | node03 | Proxmox | Tertiary |
| 192.168.20.31 | Synology NAS | Storage | Main NAS |
| 192.168.20.50 | k8s-cp-01 | K8s | Control plane |
| ... | ... | ... | ... |

### Reserved Ranges

| Range | Purpose |
|-------|---------|
| .1-.19 | Network infrastructure |
| .20-.30 | Proxmox nodes |
| .31-.40 | Storage devices |
| .50-.60 | K8s control plane |
| .61-.80 | K8s workers |
| .200-.254 | DHCP pool |

### Next Available IPs

✅ **Recommended**: 192.168.20.[X]

Available in sequence:
1. 192.168.20.[X]
2. 192.168.20.[Y]
3. 192.168.20.[Z]

### Usage Summary

- **Total IPs**: 254
- **Allocated**: [X]
- **Reserved**: [X]
- **Available**: [X]
- **Utilization**: [X]%
```

### Output Format - Check IP

```markdown
# 🔍 IP Check: 192.168.20.100

**Status**: ✅ Available / 🔴 In Use

## If In Use:
| Property | Value |
|----------|-------|
| Device | [Name] |
| Type | [VM/LXC/Physical] |
| Purpose | [Description] |
| Added | [Date] |

## If Available:
This IP is available for use.

**VLAN**: 20 - Homelab
**Subnet**: 192.168.20.0/24
**Gateway**: 192.168.20.1
**DNS**: 192.168.90.53
```

### Output Format - All VLANs Summary

```markdown
# 🌐 IP Address Summary - All VLANs

| VLAN | Name | Subnet | Allocated | Available | Util% |
|------|------|--------|-----------|-----------|-------|
| 1 | Default | 192.168.0.0/24 | [X] | [Y] | [Z]% |
| 10 | Internal | 192.168.10.0/24 | [X] | [Y] | [Z]% |
| 20 | Homelab | 192.168.20.0/24 | [X] | [Y] | [Z]% |
| 30 | IoT | 192.168.30.0/24 | [X] | [Y] | [Z]% |
| ... | ... | ... | ... | ... | ... |

## Recommendations

- VLAN with most space: [Name]
- Suggested for new VMs: VLAN 20 (Homelab)
- Suggested for IoT: VLAN 30 (IoT)
```

### Update IP Map

When reserving an IP, suggest adding to `10 - IP Address Map.md`:
```markdown
| 192.168.20.[X] | [New Service] | [Type] | [Notes] |
```

## Arguments

- `/ip-find` - Show all VLANs summary
- `/ip-find vlan20` or `/ip-find homelab` - Specific VLAN
- `/ip-find check 192.168.20.100` - Check if IP is in use
- `/ip-find next` - Next available in default VLAN (20)
- `/ip-find next vlan30` - Next available in specific VLAN
- `/ip-find reserve 192.168.20.100 "Service Name"` - Reserve and document
- `/ip-find search [term]` - Search allocations by name
