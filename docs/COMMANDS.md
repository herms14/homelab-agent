# Command Reference

> Complete reference for all Homelab Agent commands (v1.1.0)

Every command reads `CLAUDE.md` first, then looks up docs by **role** from its Documentation Structure table (Network, Proxmox, Services, IP Map, Changelog, Task Registry, ...). Roles you haven't configured are skipped. The **Sensitive** role is never read.

---

## Command Overview

| Command | Category | Purpose |
|---------|----------|---------|
| `/homelab` | Core | Main menu, summary, open tasks, alerts |
| `/lab-status` | Core | Infrastructure status report (docs or live) |
| `/service-list` | Core | Service catalog |
| `/ip-find` | Management | IP allocation, conflicts, reservations |
| `/deploy-new` | Management | Deployment code + doc checklist |
| `/capacity` | Management | Headroom and placement |
| `/troubleshoot` | Operations | Known issues, gotchas, diagnostics |
| `/lab-changelog` | Operations | Change log + task closure |
| `/runbook` | Operations | Operational procedures |
| `/doc-sync` | Operations | Documentation verification and fixes |

---

## Shared Conventions

| Convention | Used by | Defined in |
|------------|---------|------------|
| Doc roles | All commands | `CLAUDE.md` → Documentation Structure |
| Change Documentation Protocol | deploy-new, ip-find, lab-changelog, runbook, doc-sync, troubleshoot | `CLAUDE.md` |
| Task Registry | homelab, lab-status, deploy-new, lab-changelog, runbook, doc-sync | `CLAUDE.md` + `templates/Task Registry.md` |
| Retired Components | homelab, lab-status, ip-find, capacity, doc-sync | `CLAUDE.md` |
| Known Gotchas | homelab, ip-find, deploy-new, troubleshoot, runbook | `CLAUDE.md` |
| Live data (read-only) | lab-status, capacity, doc-sync | Optional SSH to a Proxmox node |

---

## Core Commands

### /homelab

**Main menu and infrastructure dashboard**

```bash
/homelab              # Main menu
/homelab quick        # Just the command list
/homelab alerts       # Alerts only
/homelab tasks        # Task Registry summary only
```

**Shows:** node/VM/LXC/service/VLAN counts, Task Registry counts, quick actions, alerts (recent changes, blocked or stale tasks, stale docs, capacity, relevant gotchas). Retired components show as `n/a`.

---

### /lab-status

**Comprehensive infrastructure status report**

```bash
/lab-status           # Full report
/lab-status proxmox   # Cluster only
/lab-status guests    # VM / LXC inventory
/lab-status k8s       # Kubernetes (if active)
/lab-status services  # Services only
/lab-status storage   # Storage and backups
/lab-status network   # Network only
/lab-status tasks     # Open tasks
/lab-status quick     # Summary only
/lab-status live      # Force read-only pvesh/pvecm queries
```

**Sections:** Proxmox health and quorum, guest inventory, Kubernetes (omitted if retired), services by category, storage and last backup, network, utilization bars, open tasks, alerts, recent activity, recommendations. States whether data came from docs or live.

---

### /service-list

**Service catalog**

```bash
/service-list                    # All services
/service-list [category]         # e.g. media, core, monitoring
/service-list host [name]        # Services on one host
/service-list search [term]      # Search
/service-list url [service]      # URL for a service
/service-list port [number]      # Service by port
```

**Columns:** service, URL, host, type, port, auth. Also lists stopped/retired services and statistics. Never prints API keys.

---

## Management Commands

### /ip-find

**IP address management**

```bash
/ip-find                              # All VLANs summary
/ip-find vlan20                       # Specific VLAN
/ip-find check 10.0.20.100            # Is it used?
/ip-find next                         # Next free in default VLAN
/ip-find next vlan40                  # Next free in a VLAN
/ip-find reserve 10.0.20.100 "Name"   # Reserve and document
/ip-find search [term]                # Search by device
/ip-find conflicts                    # Duplicate allocations only
```

**Rules:** an IP counts as used if any doc claims it or it's listed in Known Gotchas; IPs of Retired Components are free; duplicates are flagged. Reserving follows the Change Documentation Protocol.

---

### /deploy-new

**Deployment code generator**

```bash
/deploy-new                           # Wizard
/deploy-new docker [name]             # Docker on existing host
/deploy-new lxc [name]                # LXC container
/deploy-new vm [name]                 # New VM
/deploy-new k8s [name]                # Kubernetes
/deploy-new [name] --image [img:tag]
/deploy-new [name] --port [port]
/deploy-new [name] --host [host]
```

**Generates:** `pct create` or Terraform, Docker Compose (with log rotation), reverse proxy route, SSO steps, DNS, monitoring, doc updates by role, verification checklist, rollback. Checks the Task Registry first and marks the task in progress. Uses env vars, never real secrets.

---

### /capacity

**Capacity planning**

```bash
/capacity                    # Full report
/capacity plan 4cpu 8gb      # Does it fit, and where?
/capacity proxmox
/capacity k8s
/capacity storage
/capacity network
/capacity forecast           # Projection from Changelog growth
/capacity node [name]
```

**Includes:** per-node cores/RAM allocation, largest consumers, storage pools, free IPs, placement advice (including "use an LXC instead"), thresholds 🟢 <70% · 🟡 70-85% · 🔴 >85%.

---

## Operations Commands

### /troubleshoot

**Troubleshooting assistant**

```bash
/troubleshoot [description]
/troubleshoot docker | network | proxmox | k8s | auth
/troubleshoot [service-name]
/troubleshoot add
/troubleshoot diag [service]
```

**Approach:** Known Gotchas and Troubleshooting doc first, then recent Changelog entries for the affected thing, then read-only diagnostics, then fixes (with confirmation). Ships a table of generic gotchas (stale ARP, duplicate IPs, unrotated logs, NFS mount options, SSO redirect loops, lost quorum). Offers to record new issues and gotchas.

---

### /lab-changelog

**Change tracking**

```bash
/lab-changelog                    # Recent entries
/lab-changelog add "Description"
/lab-changelog add                # Interactive
/lab-changelog today | week | month
/lab-changelog search [term]
/lab-changelog stats
/lab-changelog detect             # Find undocumented changes
/lab-changelog export
```

**Categories:** Added, Changed, Fixed, Removed, Infrastructure, Security. Appends to today's heading, lists Protocol follow-ups, offers to close the matching Task Registry item, and suggests adding big removals to Retired Components.

---

### /runbook

**Runbook generator**

```bash
/runbook
/runbook maintenance [node]
/runbook backup
/runbook dr
/runbook service-restart [svc]
/runbook upgrade [component]
/runbook patching
/runbook scale [component]
/runbook network [change]
/runbook decommission [target]
/runbook security [task]
/runbook custom [title]
```

**Sections:** overview, prerequisites (backup, Task Registry check), pre-checks, procedure, verification, rollback, troubleshooting, post-procedure Protocol checklist. Can save to your Runbooks location.

---

### /doc-sync

**Documentation verification**

```bash
/doc-sync                # Full report
/doc-sync services | ips | guests
/doc-sync retired        # Mentions of retired components
/doc-sync tasks          # Task Registry hygiene
/doc-sync stale
/doc-sync live           # Compare with read-only pvesh/pct output
/doc-sync fix            # Suggest fixes
/doc-sync apply          # Apply with confirmation
/doc-sync [filename]
```

**Checks:** missing entries, duplicate IPs, guest inventory, cross-doc consistency (including `CLAUDE.md`), retired-but-mentioned, changelog coverage, stale tasks, freshness. Never opens the Sensitive doc.

---

## Cheat Sheet

```bash
# Morning
/homelab
/lab-status quick

# New service
/capacity plan 2cpu 4gb
/ip-find next vlan40
/deploy-new lxc my-service
/lab-changelog add "Deployed my-service"

# Something broke
/troubleshoot my-service returns 502
/troubleshoot diag my-service

# Monthly hygiene
/doc-sync
/doc-sync retired
/runbook patching
```

---

## Getting Help

1. Check the Documentation Structure paths in `CLAUDE.md`
2. Make sure the docs exist and use the role names
3. Open an [issue](https://github.com/herms14/homelab-agent/issues)
