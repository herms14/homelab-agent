# Changelog

All notable changes to Homelab Agent are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioning follows [SemVer](https://semver.org/).

## [1.1.0] - 2026-10-06

Synced with the agent as it is used day to day on the author's homelab, and made fully generic.

### Added
- **Doc roles**: `CLAUDE.md.template` now has a Documentation Structure table mapping roles (Network, Proxmox, Services, IP Map, Changelog, Task Registry, Sensitive, ...) to your files. Commands look up docs by role instead of hardcoded paths.
- **Change Documentation Protocol** in the template: Changelog entry, owning doc, `updated:` frontmatter, Task Registry, after every change.
- **Task Registry** for multi-session coordination, plus `templates/Task Registry.md`.
- `templates/Homelab Changelog.md` starter.
- **Retired Components** and **Known Gotchas** sections in the template, used across commands.
- **Sensitive** role: commands never open or print credentials.
- Optional read-only **live data** (`pvesh`, `pvecm`, `pct config`) for `/lab-status`, `/capacity`, `/doc-sync`.
- New arguments:
  - `/homelab tasks`
  - `/lab-status guests | tasks | live`
  - `/service-list host [name]`
  - `/ip-find conflicts`
  - `/deploy-new --host`
  - `/lab-changelog detect`
  - `/runbook patching | decommission`
  - `/doc-sync guests | retired | tasks | live`
- `/troubleshoot`: generic gotcha table (stale ARP, duplicate IPs, unrotated Docker logs, NFS options, SSO loops, lost quorum) and a check of recent Changelog entries.
- `/deploy-new`: `pct create` LXC path, Docker log rotation, Task Registry check, doc-role update table.
- `CHANGELOG.md`.
- README: "How It Works", example outputs, and "In the Wild" section.

### Changed
- All ten commands rewritten to be environment-agnostic: no personal paths, domains, IPs, or hostnames. Examples use `[placeholders]` and `10.0.x.x`.
- Kubernetes is optional everywhere. Sections are omitted when it's missing or retired.
- `/lab-status` and `/capacity` include LXC guests and RAM allocation per node.
- `/ip-find` treats IPs from Known Gotchas as unusable and IPs from Retired Components as free.
- `/lab-changelog` appends to today's heading, lists Protocol follow-ups, and can close Task Registry items.
- `/doc-sync` checks cross-doc consistency, retired-but-mentioned components, Task Registry hygiene, and no longer "verifies credentials".
- Install docs recommend cloning beside your docs (`.homelab-agent/`) and copying `commands/` into `.claude/commands/`, so existing `.claude/` settings are untouched.
- `docs/COMMANDS.md`, `docs/CUSTOMIZATION.md`, `docs/INSTALLATION.md` updated for the above.

### Removed
- Hardcoded owner-specific file paths, domain, and IP addresses from command files.

## [1.0.0] - 2026-01-16

### Added
- Initial release: `/homelab`, `/lab-status`, `/service-list`, `/ip-find`, `/deploy-new`, `/troubleshoot`, `/lab-changelog`, `/runbook`, `/capacity`, `/doc-sync`.
- `CLAUDE.md.template`, installation, command, and customization docs.
