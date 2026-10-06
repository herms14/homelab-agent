# Customization Guide

> How to adapt Homelab Agent to your infrastructure

---

## Overview

You customize the agent in three places, in order of how often you should need them:

1. **`CLAUDE.md`** - Describes your environment and where your docs live (almost everything)
2. **Your docs** - The agent is only as good as the markdown it reads
3. **Command files** - Change behavior or output format, or add new commands

Command files contain no paths, IPs, or domains. They ask for docs by **role**, and `CLAUDE.md` answers. That way you can update the commands from upstream without losing your configuration.

---

## CLAUDE.md Sections

Start from [`CLAUDE.md.template`](../CLAUDE.md.template). Sections, and which commands use them:

| Section | Purpose | Used by |
|---------|---------|---------|
| Overview | Cluster name, domain, timezone, SSH user | All |
| Infrastructure Summary | Counts at a glance | homelab, lab-status |
| **Documentation Structure** | Maps roles to file paths | All |
| **Change Documentation Protocol** | What to update after any change | deploy-new, ip-find, lab-changelog, runbook, doc-sync |
| **Task Coordination** | Multi-session rules | homelab, deploy-new, lab-changelog, runbook, doc-sync |
| Network Architecture | VLANs, default VLAN, DNS, IP conventions | ip-find, deploy-new, capacity |
| Proxmox Cluster | Nodes, storage, backups | lab-status, capacity, runbook |
| Service Conventions | URL pattern, SSO, Docker paths | deploy-new, service-list |
| Automation | Terraform / Ansible / CI | deploy-new |
| **Retired Components** | What no longer exists | homelab, lab-status, ip-find, capacity, doc-sync |
| **Known Gotchas** | Traps the agent must respect | troubleshoot, ip-find, deploy-new, runbook |
| Behavioral Rules | Safety and style rules | All |

### Documentation Structure (doc roles)

| Role | Typical content |
|------|-----------------|
| Index | Overview / navigation |
| Network | VLANs, topology, DNS, remote access |
| Proxmox | Nodes, VMs, LXCs, templates |
| Storage | NAS, NFS, pools, backups |
| Kubernetes | Optional |
| Services | Service catalog |
| IP Map | IP allocations |
| Reverse Proxy | Traefik / Caddy / NPM conventions |
| SSO | Authentik / Authelia |
| Monitoring | Prometheus, Grafana, uptime |
| Automation | Terraform / Ansible / CI |
| Onboarding | Your new-service workflow |
| Troubleshooting | Known issues |
| Runbooks | Folder for saved runbooks |
| Changelog | Change log |
| Task Registry | Session coordination |
| Sensitive | Credentials. Listed so the agent **never** reads it |

Keep the role names as-is; change only the paths. Several roles can point at the same file if your docs are consolidated.

### Retired Components

```markdown
| Component | Retired | Notes |
|-----------|---------|-------|
| Kubernetes cluster | 2026-05-01 | 9 VMs deleted, IPs 10.0.20.50-.70 freed |
| Hyper-V host | 2026-03-12 | Workloads moved to Proxmox |
```

Effects: dashboards show `n/a`, `/ip-find` treats the IPs as free, `/doc-sync retired` finds docs that still describe them as active.

### Known Gotchas

One line each, specific and actionable:

```markdown
- NAS eth0 (10.0.20.30) is dead. Always use eth1 (10.0.20.31) for NFS, UI, SNMP.
- After recreating an LXC, flush ARP on every node: `ip neigh flush dev vmbr0 <IP>`.
- GitLab VM disk filled once from container logs; keep Docker log rotation on.
- Never auto-patch the backup server or the Ansible controller.
```

`/troubleshoot add` will offer to append new ones.

---

## Change Documentation Protocol and Task Registry

These two conventions keep docs truthful when Claude makes changes for you, and keep parallel sessions from colliding.

**Protocol (after every change):** Changelog entry → update the doc that owns the fact → bump `updated:` frontmatter → update the Task Registry.

**Task Registry:** a markdown file with In Progress / Blocked / Pending / Completed tables. Sessions check it before starting, mark tasks `🔄 In Progress` with a timestamp, and close them with `✅ Completed` and a note. Starters for both files are in [`templates/`](../templates/).

Don't want them? Remove the Changelog and Task Registry rows from Documentation Structure and delete the two sections from `CLAUDE.md`. Commands skip what isn't configured.

---

## Documentation Layout Options

### Flat

```
docs/
├── CLAUDE.md
├── network.md
├── proxmox.md
├── services.md
├── ip-map.md
└── troubleshooting.md
```

### Folders

```
docs/
├── CLAUDE.md
├── infrastructure/{network,proxmox,storage}.md
├── services/catalog.md
└── operations/{ip-map,troubleshooting}.md
```

### Numbered Notes (Obsidian style)

```
Homelab/
├── CLAUDE.md
├── 00 - Index.md
├── 01 - Network Architecture.md
├── 02 - Proxmox Cluster.md
├── 07 - Deployed Services.md
├── 10 - IP Address Map.md
├── 12 - Troubleshooting.md
├── Homelab Changelog.md
└── Task Registry.md
```

In every case, only the Documentation Structure table changes.

---

## Customizing Commands

### Modify Existing Commands

Edit `.claude/commands/[command].md`. Common tweaks:

- Add a service category in `service-list.md`
- Add runbook types in `runbook.md`
- Change capacity thresholds in `capacity.md`
- Replace the Traefik/Authentik examples in `deploy-new.md` with Caddy/Authelia

Keep personal values (IPs, domain, hostnames) in `CLAUDE.md`, not in command files, so upstream updates merge cleanly.

### Create a Custom Command

Add a new `.md` file to `.claude/commands/`:

```markdown
# My Command

One-line description.

## Instructions

What Claude should do.

### Data Sources

Read `CLAUDE.md` first. Then, using its Documentation Structure table, read:
- **Services** - ...
- **IP Map** - ...

Never open the doc listed under the **Sensitive** role.

### Output Format

[template]

## Arguments

- `/my-command` - Default behavior
- `/my-command [arg]` - With argument
```

If your command changes anything, end it with the Change Documentation Protocol steps.

---

## Examples

### Single Node, Docker Only

Fill in Overview, Network, Services, IP Map. Leave out Kubernetes, Automation, Task Registry. `/capacity` and `/lab-status` adapt to one node.

### Lab That Retired Kubernetes

Add Kubernetes to Retired Components. Every K8s section disappears from reports, and `/ip-find` reclaims its IPs.

### Multi-Site

Add a `## Sites` section to `CLAUDE.md` with each site's subnets and link type (WireGuard, Tailscale). Commands will include the site in IP and placement answers.

---

## Tips

1. **Be specific** - The more facts in docs, the fewer guesses
2. **One owner per fact** - Each fact lives in one doc; others link to it
3. **Record removals** - Retired Components matters as much as inventory
4. **Write down every trap** - Known Gotchas saves you from repeat outages
5. **Run `/doc-sync` monthly**
