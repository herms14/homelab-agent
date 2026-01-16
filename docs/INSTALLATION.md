# Installation Guide

> Complete guide to installing and configuring Homelab Agent

## Prerequisites

Before installing, ensure you have:

- ✅ **Claude Code** installed ([Get it here](https://claude.ai/code))
- ✅ **Homelab documentation** in markdown format
- ✅ **Git** (optional, for cloning)
- ✅ Basic command line familiarity

---

## Installation Methods

### Method 1: Git Clone (Recommended)

```bash
# Navigate to your documentation root
cd "path/to/your/homelab/docs"

# Clone the repository into .claude folder
git clone https://github.com/herms14/homelab-agent .claude

# Copy CLAUDE.md template to root
cp .claude/CLAUDE.md.template ./CLAUDE.md
```

### Method 2: Manual Download

1. **Download** the repository as ZIP from GitHub
2. **Extract** the contents
3. **Copy** the `commands/` folder to `.claude/commands/` in your docs
4. **Copy** `CLAUDE.md.template` to your docs root as `CLAUDE.md`

### Method 3: Copy Individual Commands

If you only want specific commands:

1. Create `.claude/commands/` folder in your docs
2. Copy only the command files you want:
   - `homelab.md` - Main menu
   - `lab-status.md` - Status reports
   - `service-list.md` - Service catalog
   - etc.

---

## Folder Structure After Installation

Your documentation should look like this:

```
your-homelab-docs/
├── .claude/
│   ├── commands/
│   │   ├── homelab.md
│   │   ├── lab-status.md
│   │   ├── service-list.md
│   │   ├── ip-find.md
│   │   ├── deploy-new.md
│   │   ├── troubleshoot.md
│   │   ├── lab-changelog.md
│   │   ├── runbook.md
│   │   ├── capacity.md
│   │   └── doc-sync.md
│   └── settings.local.json
├── CLAUDE.md                    # ⬅️ Important! Must be in docs root
├── network.md                   # Your network documentation
├── services.md                  # Your service catalog
├── ips.md                       # Your IP allocations
└── ... (your other docs)
```

---

## Configuration

### Step 1: Customize CLAUDE.md

Edit `CLAUDE.md` in your docs root. This file tells Claude about your infrastructure:

```markdown
# Homelab Context

## Infrastructure Overview

**Cluster Name**: YourClusterName
**Domain**: yourdomain.xyz
**Primary VLAN**: 20 (192.168.20.0/24)

## Proxmox Nodes

| Node | IP | Specs |
|------|-----|-------|
| node01 | 192.168.20.20 | 24 cores, 64GB RAM |
| node02 | 192.168.20.21 | 20 cores, 48GB RAM |

## Documentation Structure

| File | Purpose |
|------|---------|
| `network.md` | Network architecture, VLANs |
| `proxmox.md` | Proxmox cluster details |
| `services.md` | Deployed services catalog |
| `ips.md` | IP address allocations |
| `troubleshooting.md` | Known issues and fixes |

## Conventions

- IP Range for VMs: 192.168.20.100-199
- Service URLs: https://[service].yourdomain.xyz
- Auth: Authentik SSO for most services
```

### Step 2: Configure Permissions (Optional but Recommended)

To avoid permission prompts, add to `~/.claude/settings.json`:

**Windows**: `%USERPROFILE%\.claude\settings.json`
**Mac/Linux**: `~/.claude/settings.json`

```json
{
  "permissions": {
    "allow": [
      "Read(path/to/your/docs/**)",
      "Write(path/to/your/docs/**)",
      "Edit(path/to/your/docs/**)",
      "Glob(**)",
      "Grep(**)"
    ]
  }
}
```

### Step 3: Verify Installation

```bash
# Navigate to docs
cd "path/to/your/docs"

# Start Claude Code
claude

# Test the installation
> /homelab
```

You should see the main menu with all available commands.

---

## Documentation Requirements

The agent works best when your documentation includes:

### Required Files (Recommended)

| File | What to Include |
|------|-----------------|
| **Network docs** | VLANs, subnets, IP ranges |
| **Service catalog** | All services with URLs, ports |
| **IP allocations** | Device-to-IP mappings |

### Optional Files (Enhanced Features)

| File | Enables |
|------|---------|
| **Troubleshooting guide** | `/troubleshoot` search |
| **Terraform configs** | `/deploy-new` templates |
| **Ansible playbooks** | `/deploy-new` patterns |
| **Change log** | `/lab-changelog` tracking |

### Example Documentation Structure

```
homelab-docs/
├── infrastructure/
│   ├── network-architecture.md
│   ├── proxmox-cluster.md
│   ├── kubernetes-cluster.md
│   └── storage.md
├── services/
│   ├── deployed-services.md
│   ├── media-stack.md
│   └── monitoring.md
├── operations/
│   ├── ip-address-map.md
│   ├── troubleshooting.md
│   └── changelog.md
├── automation/
│   ├── terraform/
│   └── ansible/
└── CLAUDE.md
```

---

## Platform-Specific Notes

### Windows

- Use forward slashes `/` in paths
- Settings file: `C:\Users\YourName\.claude\settings.json`
- Example path: `C:/Users/YourName/Documents/HomeLabDocs/**`

### macOS

- Settings file: `~/.claude/settings.json`
- Example path: `/Users/YourName/Documents/HomeLabDocs/**`

### Linux

- Settings file: `~/.claude/settings.json`
- Example path: `/home/YourName/docs/homelab/**`

---

## Troubleshooting Installation

### Commands Not Showing

**Problem**: Slash commands don't appear

**Solutions**:
1. Ensure you're in the docs directory when running `claude`
2. Check that `.claude/commands/` folder exists
3. Verify files have `.md` extension
4. Restart Claude Code session

### Permission Denied Errors

**Problem**: Claude can't read/write files

**Solutions**:
1. Add permissions to `~/.claude/settings.json`
2. When prompted, select "Always allow for this directory"
3. Check file/folder permissions in your OS

### CLAUDE.md Not Being Read

**Problem**: Claude doesn't know your infrastructure

**Solutions**:
1. Ensure `CLAUDE.md` is in docs ROOT (not in `.claude/`)
2. Check file isn't named `claude.md` (case matters)
3. Verify proper markdown formatting

---

## Updating

### Via Git

```bash
cd "path/to/your/docs/.claude"
git pull origin main
```

### Manual Update

1. Download latest release from GitHub
2. Replace files in `.claude/commands/`
3. Check release notes for `CLAUDE.md` changes

---

## Next Steps

After installation:

1. **Customize CLAUDE.md** for your infrastructure
2. **Run `/homelab`** to see the main menu
3. **Try `/lab-status`** for a status report
4. **Explore `/service-list`** to see your services

---

## Getting Help

- 📖 [Full Documentation](./COMMANDS.md)
- 🐛 [Report Issues](https://github.com/herms14/homelab-agent/issues)
- 💬 [Discussions](https://github.com/herms14/homelab-agent/discussions)
