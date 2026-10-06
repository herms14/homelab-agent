# Homelab Agent

Main menu and status dashboard for homelab infrastructure management.

## Instructions

Display the homelab agent welcome screen with infrastructure summary, open tasks, alerts, and available commands.

### Data Sources

Read `CLAUDE.md` first. Then use its **Documentation Structure** table to find and read these docs by role:
- **Index** - Main overview
- **Proxmox** - Nodes, VMs, LXCs
- **Services** - Service catalog
- **Network** - VLAN count
- **Task Registry** - Open / blocked tasks (if present)
- **Changelog** - Most recent entries (if present)

Never open the doc listed under the **Sensitive** role.

### Output Format

```
╭──────────────────────────────────────────────────────────────────╮
│  🏠 Homelab Agent - [Cluster Name]                               │
╰──────────────────────────────────────────────────────────────────╯

Welcome! I'm your homelab infrastructure assistant.

┌─────────────────────────────────────────────────────────────────┐
│  📊 Infrastructure Summary                                      │
├─────────────────────────────────────────────────────────────────┤
│  Proxmox Nodes: [X]    │  VMs: [X]    │  LXCs: [X]             │
│  Docker Services: [X]+ │  VLANs: [X]  │  Storage: [X] TB       │
│  K8s Nodes: [X or "n/a"]                                        │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  🗂️ Task Registry                                               │
├─────────────────────────────────────────────────────────────────┤
│  🔄 In Progress: [X]   ⏸️ Blocked: [X]   📋 Pending: [X]         │
│  • [Most recent in-progress or blocked task]                   │
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
  • [Recent changes from the Changelog]
  • [Blocked tasks from the Task Registry]
  • [Stale documentation]
  • [Capacity warnings]

💡 Tip: Run /lab-status for a comprehensive infrastructure report.

What would you like to do?
```

Omit the Task Registry box if no Task Registry doc is configured. Show `n/a` for any component listed under **Retired Components** in `CLAUDE.md`.

### Alert Detection

Check for:
1. **Recent changes** - Latest Changelog entries, plus homelab docs modified in the last 24 hours
2. **Blocked tasks** - `⏸️ Blocked` items in the Task Registry
3. **Stale tasks** - `🔄 In Progress` items older than 7 days (possibly abandoned by another session)
4. **Stale docs** - Files whose `updated:` frontmatter (or modified time) is 30+ days old
5. **Capacity** - Any resource > 80% utilized
6. **Gotchas** - Anything under **Known Gotchas** in `CLAUDE.md` that is currently relevant

### Quick Stats Extraction

From the Proxmox doc:
- Count nodes, VMs (excluding templates), LXCs
- Note cluster/quorum health

From the Services doc:
- Count total services, grouped by category

From the Network doc:
- Count VLANs

## Arguments

- `/homelab` - Show main menu with summary
- `/homelab quick` - Just show available commands
- `/homelab alerts` - Focus on alerts only
- `/homelab tasks` - Show the Task Registry summary only
