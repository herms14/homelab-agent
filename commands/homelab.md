# Homelab Agent

Main menu and status dashboard for homelab infrastructure management.

## Instructions

Display the homelab agent welcome screen with infrastructure summary and available commands.

### Data Sources

Read these files for context:
- `07 HomeLab Things/Claude Managed Homelab/00 - Homelab Index.md` - Main index
- `07 HomeLab Things/Claude Managed Homelab/02 - Proxmox Cluster.md` - Cluster info
- `07 HomeLab Things/Claude Managed Homelab/07 - Deployed Services.md` - Services

### Output Format

```
╭──────────────────────────────────────────────────────────────────╮
│  🏠 Homelab Agent - MorpheusCluster                              │
╰──────────────────────────────────────────────────────────────────╯

Welcome! I'm your homelab infrastructure assistant.

┌─────────────────────────────────────────────────────────────────┐
│  📊 Infrastructure Summary                                      │
├─────────────────────────────────────────────────────────────────┤
│  Proxmox Nodes: [X]    │  VMs: [X]    │  LXCs: [X]             │
│  K8s Nodes: [X]        │  Docker Services: [X]+                │
│  VLANs: [X]            │  Storage: [X] GB                      │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  📋 Quick Actions                                               │
├──────────────────┬──────────────────────────────────────────────┤
│  /lab-status     │  📊 Full infrastructure status report        │
│  /service-list   │  📦 List all services with URLs              │
│  /ip-find        │  🔍 Find available IP addresses              │
│  /deploy-new     │  🚀 Generate deployment code                 │
│  /troubleshoot   │  🔧 Search troubleshooting guides            │
│  /lab-changelog  │  📋 Log infrastructure changes               │
│  /runbook        │  📖 Generate operational runbook             │
│  /capacity       │  📈 Resource capacity planning               │
│  /doc-sync       │  🔄 Verify documentation accuracy            │
└──────────────────┴──────────────────────────────────────────────┘

⚠️  Alerts:
  • [List any recent changes, pending updates, or issues]
  • [Check for stale documentation]
  • [Note any capacity warnings]

💡 Tip: Run /lab-status for a comprehensive infrastructure report.

What would you like to do?
```

### Alert Detection

Check for:
1. **Recent changes** - Files modified in last 24 hours in homelab folder
2. **Stale docs** - Files not updated in 30+ days
3. **Capacity** - Note if any resource > 80% utilized
4. **TODOs** - List incomplete TODO items from index

### Quick Stats Extraction

From Proxmox Cluster doc:
- Count nodes, VMs, LXCs
- Note cluster health

From Deployed Services:
- Count total services
- Group by category

From Network Architecture:
- Count VLANs

## Arguments

- `/homelab` - Show main menu with summary
- `/homelab quick` - Just show available commands
- `/homelab alerts` - Focus on alerts only
