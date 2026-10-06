# 🏠 Homelab Agent

> **A doc-driven homelab assistant for Claude Code: ten slash commands that read your markdown docs and help you run, change, and document your lab.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-blue)](https://claude.ai/code)
[![Proxmox](https://img.shields.io/badge/Proxmox-Compatible-orange)](https://www.proxmox.com/)
[![Docker](https://img.shields.io/badge/Docker-Compatible-2496ED)](https://docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Optional-326CE5)](https://kubernetes.io/)

**Current version: 1.1.0** · See [CHANGELOG.md](CHANGELOG.md)

---

## ✨ Commands

| Command | What it does |
|---------|--------------|
| `/homelab` | Main menu: infrastructure summary, open tasks, alerts, quick actions |
| `/lab-status` | Full status report for nodes, guests, services, storage, network (docs or live `pvesh`) |
| `/service-list` | Service catalog with URLs, hosts, ports, and auth, grouped by category |
| `/ip-find` | Find free IPs per VLAN, check an IP, detect duplicate allocations, reserve and document |
| `/deploy-new` | Generate LXC / Terraform / Docker Compose / reverse proxy / SSO code plus a doc checklist |
| `/troubleshoot` | Match symptoms against your known issues, recent changes, and gotchas; give diagnostics |
| `/lab-changelog` | Add and browse changelog entries; detect undocumented changes; close matching tasks |
| `/runbook` | Generate maintenance, patching, backup, DR, decommission, and custom runbooks |
| `/capacity` | CPU / RAM / storage / IP headroom and best-node placement for a new guest |
| `/doc-sync` | Find missing, conflicting, stale, or retired-but-still-mentioned facts and fix them |

Full reference: [docs/COMMANDS.md](docs/COMMANDS.md)

---

## 🧠 How It Works

Homelab Agent is not a daemon or an API integration. It is a set of **prompt files** that Claude Code loads as slash commands. Each command tells Claude which of **your markdown docs** to read and what to produce.

```
          you type /ip-find next
                   │
                   ▼
   .claude/commands/ip-find.md   ← instructions + output format
                   │
                   ▼
        CLAUDE.md (your docs root)
   ├── Documentation Structure   ← maps roles to your files
   ├── VLANs, conventions        ← default VLAN, URL pattern, SSO...
   ├── Retired Components        ← don't report these, IPs are free
   └── Known Gotchas             ← traps to avoid (dead NICs, stale ARP...)
                   │
                   ▼
   your docs: IP Map, Proxmox, Services, Network ...
                   │
                   ▼
        answer + suggested doc edits
```

Three conventions make it work:

1. **Doc roles.** Commands never hardcode file paths. They ask for the *IP Map*, *Services*, *Proxmox*, *Changelog* doc and so on, and `CLAUDE.md` tells them where each one lives. Missing roles are skipped.
2. **Change Documentation Protocol.** Every change updates the Changelog, the doc that owns the fact, and `updated:` frontmatter, so your docs stay the source of truth.
3. **Task Registry.** A shared markdown task list that lets several Claude Code sessions work on the same lab without stepping on each other.

Optionally, commands such as `/lab-status`, `/capacity`, and `/doc-sync` can use read-only live commands (`pvesh`, `pvecm`, `pct config`) if Claude Code has shell access to a Proxmox node.

---

## 🚀 Install

### Prerequisites

- [Claude Code](https://claude.ai/code)
- Homelab documentation in markdown (Obsidian vault, Git repo, plain folder)

### Steps

1. **Copy the commands** into your docs folder:

   ```bash
   cd "path/to/your/homelab/docs"
   git clone https://github.com/herms14/homelab-agent .homelab-agent
   mkdir -p .claude/commands
   cp .homelab-agent/commands/*.md .claude/commands/
   ```

   Already have a `.claude/` folder? This copies only the command files and leaves your other settings alone.

2. **Create your context file**:

   ```bash
   cp .homelab-agent/CLAUDE.md.template ./CLAUDE.md
   ```

   Fill in the **Documentation Structure** table, VLANs, nodes, conventions, Retired Components, and Known Gotchas.

3. **(Recommended) add the Changelog and Task Registry starters**:

   ```bash
   cp ".homelab-agent/templates/Homelab Changelog.md" .
   cp ".homelab-agent/templates/Task Registry.md" .
   ```

4. **Run it**:

   ```bash
   claude
   > /homelab
   ```

Updating later: `cd .homelab-agent && git pull`, then copy `commands/*.md` again. Details and Windows notes: [docs/INSTALLATION.md](docs/INSTALLATION.md).

---

## 🖥️ Example Output

### `/homelab`

```
╭──────────────────────────────────────────────────────────────────╮
│  🏠 Homelab Agent - MyCluster                                    │
╰──────────────────────────────────────────────────────────────────╯

  Proxmox Nodes: 3 (+QDevice, quorate)  │  VMs: 5  │  LXCs: 13
  Docker Services: 30+  │  VLANs: 7  │  K8s: n/a (retired)

  🗂️ Tasks: 🔄 1 in progress  ⏸️ 1 blocked  📋 4 pending

⚠️  Alerts:
  • node02 RAM allocated at 86%
  • Blocked: "install node_exporter on node03" (waiting on reboot window)
  • services.md not updated in 41 days
```

### `/ip-find next vlan40`

```markdown
# 🔍 IP Address Finder

## VLAN 40 - Services (10.0.40.0/24)

⚠️ Conflict: 10.0.40.14 is claimed by bots-lxc and api-lxc

✅ Recommended: 10.0.40.28
Also free: 10.0.40.29, 10.0.40.30
```

### `/capacity plan 2cpu 4gb`

```markdown
# 🧮 Capacity Check: New Guest

✅ CAN DEPLOY

| Node   | RAM After | Status      |
|--------|-----------|-------------|
| node01 | 74%       | 🟡 Possible |
| node02 | 100%      | 🔴 Avoid    |
| node03 | 61%       | 🟢 Best     |

Recommendation: node03. Consider an LXC instead of a VM to save RAM.
```

### `/lab-changelog add "Deployed paperless on utility VM"`

```markdown
## [2026-10-06] - Deployed paperless

### Added
- ✅ New service: Paperless-ngx on utility-vm01 - document management
  - File or resource affected: `services.md`

Protocol follow-ups:
- [x] Services doc updated
- [x] updated: frontmatter bumped
- [x] Task Registry: "Deploy paperless" → ✅ Completed
```

---

## 🌍 In the Wild

Homelab Agent runs the author's own homelab every day: a **3-node Proxmox VE cluster with a QDevice, about 18 guests** (a handful of VMs and a dozen LXCs) running Traefik, Authentik SSO, Pi-hole, GitLab with CI runners, Immich, a media stack, Home Assistant, Prometheus/Grafana, and a set of custom dashboards and APIs. The docs it reads live in an Obsidian vault, and the same Changelog + Task Registry pattern shipped here keeps several Claude Code sessions in sync.

- 📚 Infrastructure docs: [herms14/homelab-infrastructure](https://github.com/herms14/homelab-infrastructure)
- ✍️ Blog, Clustered Thoughts: [herms14.github.io/Clustered-Thoughts](https://herms14.github.io/Clustered-Thoughts/)

The lab has changed a lot since v1.0 (Kubernetes and a hybrid cloud lab were retired, most services moved to LXCs). That is why v1.1 added **Retired Components** and **Known Gotchas**: docs have to describe what is gone and what bites, not only what exists.

---

## ⚙️ Customization

Most customization happens in `CLAUDE.md`, not in the command files:

| Want to... | Edit |
|------------|------|
| Point commands at your files | `Documentation Structure` table |
| Change default VLAN / URL pattern / SSO | `Network Architecture`, `Service Conventions` |
| Stop reporting Kubernetes (or anything else) | `Retired Components` |
| Teach the agent a trap to avoid | `Known Gotchas` |
| Keep secrets out of reach | `Sensitive` role (never read) |

See [docs/CUSTOMIZATION.md](docs/CUSTOMIZATION.md) for layouts (flat, folders, numbered Obsidian notes) and writing your own commands.

---

## 📁 Repository Layout

```
homelab-agent/
├── commands/                 # Slash commands → copy to .claude/commands/
│   ├── homelab.md
│   ├── lab-status.md
│   ├── service-list.md
│   ├── ip-find.md
│   ├── deploy-new.md
│   ├── troubleshoot.md
│   ├── lab-changelog.md
│   ├── runbook.md
│   ├── capacity.md
│   └── doc-sync.md
├── templates/                # Starter docs the commands maintain
│   ├── Homelab Changelog.md
│   └── Task Registry.md
├── docs/
│   ├── INSTALLATION.md
│   ├── COMMANDS.md
│   └── CUSTOMIZATION.md
├── CLAUDE.md.template        # → copy to your docs root as CLAUDE.md
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

---

## 🔒 Safety

- Commands are told never to open the doc you mark as **Sensitive** and never to print secrets.
- Live commands are read-only. Anything destructive (`destroy`, `rm -rf`) requires your confirmation.
- Nothing leaves your machine except what Claude Code itself sends to the model.

---

## 🤝 Contributing

PRs welcome. Keep commands generic: no real IPs, hostnames, domains, or secrets; use doc roles and `[placeholders]`. See [CONTRIBUTING.md](CONTRIBUTING.md).

Ideas: more runbook templates, Proxmox Backup Server checks, monitoring API integrations, Unraid/TrueNAS variants.

---

## 📄 License

MIT. See [LICENSE](LICENSE).

---

## 📬 Support

- 🐛 [GitHub Issues](https://github.com/herms14/homelab-agent/issues)
- 💬 [GitHub Discussions](https://github.com/herms14/homelab-agent/discussions)
- 🏠 r/homelab, r/selfhosted

<p align="center">
  Made with ❤️ for the homelab community
</p>
