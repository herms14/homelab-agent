# Installation Guide

> Install and configure Homelab Agent

## Prerequisites

- ✅ **Claude Code** installed ([Get it here](https://claude.ai/code))
- ✅ **Homelab documentation** in markdown format (Obsidian vault, Git repo, or plain folder)
- ✅ **Git** (optional, for cloning and updating)

---

## Installation Methods

### Method 1: Clone Beside Your Docs (Recommended)

Keeps the agent repo separate from your own `.claude/` settings, so updates never overwrite them.

```bash
cd "path/to/your/homelab/docs"
git clone https://github.com/herms14/homelab-agent .homelab-agent

mkdir -p .claude/commands
cp .homelab-agent/commands/*.md .claude/commands/
cp .homelab-agent/CLAUDE.md.template ./CLAUDE.md

# Optional but recommended: starter docs the commands maintain
cp ".homelab-agent/templates/Homelab Changelog.md" .
cp ".homelab-agent/templates/Task Registry.md" .
```

Windows PowerShell equivalent:

```powershell
cd "C:\path\to\your\homelab\docs"
git clone https://github.com/herms14/homelab-agent .homelab-agent
New-Item -ItemType Directory -Force .claude\commands | Out-Null
Copy-Item .homelab-agent\commands\*.md .claude\commands\
Copy-Item .homelab-agent\CLAUDE.md.template .\CLAUDE.md
Copy-Item ".homelab-agent\templates\Homelab Changelog.md", ".homelab-agent\templates\Task Registry.md" .
```

If your docs folder is itself a Git repo, add `.homelab-agent/` to its `.gitignore`.

### Method 2: Clone as `.claude` (Fresh Docs Folder Only)

If you have no existing `.claude/` folder, you can clone straight into it. Claude Code finds `commands/` inside it.

```bash
cd "path/to/your/homelab/docs"
git clone https://github.com/herms14/homelab-agent .claude
cp .claude/CLAUDE.md.template ./CLAUDE.md
```

### Method 3: Manual Download

1. Download the repository ZIP from GitHub and extract it
2. Copy `commands/*.md` to `.claude/commands/` in your docs
3. Copy `CLAUDE.md.template` to your docs root as `CLAUDE.md`
4. Optionally copy the files in `templates/` to your docs

### Method 4: Pick Individual Commands

Copy only the command files you want into `.claude/commands/`. Each command works on its own; they just reference each other in suggestions.

---

## Folder Structure After Installation

```
your-homelab-docs/
├── .claude/
│   └── commands/
│       ├── homelab.md
│       ├── lab-status.md
│       ├── service-list.md
│       ├── ip-find.md
│       ├── deploy-new.md
│       ├── troubleshoot.md
│       ├── lab-changelog.md
│       ├── runbook.md
│       ├── capacity.md
│       └── doc-sync.md
├── .homelab-agent/              # Method 1 only (for updates)
├── CLAUDE.md                    # ⬅️ Must be in the docs root
├── Homelab Changelog.md         # Optional starter
├── Task Registry.md             # Optional starter
├── network.md                   # Your docs...
├── services.md
├── ip-map.md
└── ...
```

---

## Configuration

### Step 1: Fill In CLAUDE.md

The most important section is **Documentation Structure**. Commands look up docs by *role*, so map each role to your actual file:

```markdown
| Role | Path | Purpose |
|------|------|---------|
| Network | `infrastructure/network.md` | VLANs, DNS |
| Proxmox | `infrastructure/proxmox.md` | Nodes, VMs, LXCs |
| Services | `services/catalog.md` | Service catalog |
| IP Map | `operations/ip-map.md` | IP allocations |
| Troubleshooting | `operations/troubleshooting.md` | Known issues |
| Changelog | `Homelab Changelog.md` | Change log |
| Task Registry | `Task Registry.md` | Session coordination |
| Sensitive | `secrets/credentials.md` | Never read |
```

Then fill in VLANs, nodes, conventions, **Retired Components**, and **Known Gotchas**. See [CUSTOMIZATION.md](./CUSTOMIZATION.md).

### Step 2: Permissions (Optional)

To reduce permission prompts, add to your Claude Code settings (`~/.claude/settings.json`, or `%USERPROFILE%\.claude\settings.json` on Windows):

```json
{
  "permissions": {
    "allow": [
      "Read(path/to/your/docs/**)",
      "Edit(path/to/your/docs/**)",
      "Write(path/to/your/docs/**)",
      "Glob(**)",
      "Grep(**)"
    ],
    "deny": [
      "Read(path/to/your/docs/secrets/**)"
    ]
  }
}
```

A `deny` rule on your credentials file backs up the **Sensitive** role in `CLAUDE.md`.

### Step 3: Live Data (Optional)

`/lab-status live`, `/capacity`, and `/doc-sync live` can run read-only commands on a Proxmox node if Claude Code can reach one over SSH (for example `ssh node01 pvesh get /cluster/resources`). Set up key-based SSH yourself; the agent never needs passwords. Without SSH, everything works from docs alone.

### Step 4: Verify

```bash
cd "path/to/your/docs"
claude
> /homelab
```

You should see the main menu with your infrastructure summary.

---

## Documentation Requirements

### Core (recommended)

| Role | What to Include |
|------|-----------------|
| **Network** | VLANs, subnets, gateway, DNS |
| **Services** | Every service with URL, host, port, auth |
| **IP Map** | Device-to-IP mappings |
| **Proxmox** | Nodes, VMs, LXCs, templates |

### Optional (unlock more features)

| Role | Enables |
|------|---------|
| **Troubleshooting** | `/troubleshoot` known-issue matching |
| **Automation** / **Onboarding** | `/deploy-new` follows your patterns |
| **Storage** | `/capacity` storage section |
| **Changelog** | `/lab-changelog`, recent-activity alerts |
| **Task Registry** | Multi-session coordination, task alerts |
| **Runbooks** | `/runbook` saves and reuses procedures |

---

## Platform Notes

| OS | Settings file | Path style in permissions |
|----|---------------|---------------------------|
| Windows | `C:\Users\You\.claude\settings.json` | `C:/Users/You/Docs/Homelab/**` |
| macOS | `~/.claude/settings.json` | `/Users/you/Docs/Homelab/**` |
| Linux | `~/.claude/settings.json` | `/home/you/docs/homelab/**` |

---

## Troubleshooting Installation

### Commands Not Showing
1. Start `claude` from the docs root
2. Check `.claude/commands/*.md` exists (not `.claude/commands/commands/`)
3. Restart the Claude Code session

### Commands Can't Find Your Docs
1. Check the paths in the **Documentation Structure** table
2. Paths are relative to the folder containing `CLAUDE.md`
3. Keep the role names unchanged (Network, Services, IP Map, ...)

### CLAUDE.md Not Being Read
1. It must be in the docs ROOT, not inside `.claude/`
2. The name is case-sensitive: `CLAUDE.md`

---

## Updating

### Method 1 installs

```bash
cd "path/to/your/docs/.homelab-agent"
git pull
cp commands/*.md ../.claude/commands/
```

### Method 2 installs

```bash
cd "path/to/your/docs/.claude"
git pull
```

After updating, read [CHANGELOG.md](../CHANGELOG.md) and compare `CLAUDE.md.template` with your `CLAUDE.md` for new sections (v1.1.0 added Change Documentation Protocol, Task Coordination, Retired Components, Known Gotchas, and doc roles).

---

## Getting Help

- 📖 [Command Reference](./COMMANDS.md)
- 🐛 [Report Issues](https://github.com/herms14/homelab-agent/issues)
- 💬 [Discussions](https://github.com/herms14/homelab-agent/discussions)
