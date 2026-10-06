# Doc Sync

Verify documentation accuracy and find discrepancies.

## Instructions

Check whether homelab documentation is internally consistent and matches the actual infrastructure state.

### Data Sources

Read `CLAUDE.md` first, then every doc in its **Documentation Structure** table **except the Sensitive role**. Never open, quote, or "verify" credential files; only report that they exist.

### Live Comparison (optional)

If the user has shell access to a Proxmox node, compare docs with reality using read-only commands:

```bash
pvesh get /cluster/resources --type vm --output-format json   # Guests, status, node
pct config [ctid] | grep net0                                 # LXC IPs
qm config [vmid] | grep ipconfig                              # VM IPs (cloud-init)
pveversion                                                    # Proxmox version
```

On Docker hosts: `docker ps --format '{{.Names}} {{.Ports}}'`.

### Checks to Perform

1. **Service Catalog** - Every running service documented; no documented service that no longer exists
2. **IP Addresses** - No duplicates; every guest IP present in the IP Map; IP Map matches Proxmox doc
3. **Guest Inventory** - VM / LXC lists match (IDs, names, nodes, running vs stopped)
4. **Cross-Doc Consistency** - Same fact (IP, port, node, version) stated the same way everywhere, including `CLAUDE.md`
5. **Retired Components** - Nothing listed as retired is still described as active elsewhere
6. **Changelog Coverage** - Recent doc edits or detected changes have Changelog entries
7. **Task Registry Hygiene** - `🔄 In Progress` items older than 7 days, completed work not marked done
8. **Freshness** - `updated:` frontmatter (or modified time) older than 30/60/90 days

### Output Format

```markdown
# 🔄 Documentation Sync Report

**Generated**: [Date]
**Files Checked**: [X]
**Source**: Docs only / Docs + Live
**Issues Found**: [X]

---

## 📊 Summary

| Category | Status | Issues |
|----------|--------|--------|
| Service Catalog | 🟢 / 🟡 / 🔴 | [X] |
| IP Addresses | 🟢 / 🟡 / 🔴 | [X] |
| Guest Inventory | 🟢 / 🟡 / 🔴 | [X] |
| Cross-Doc Consistency | 🟢 / 🟡 / 🔴 | [X] |
| Retired Components | 🟢 / 🟡 / 🔴 | [X] |
| Changelog / Task Registry | 🟢 / 🟡 / 🔴 | [X] |
| Freshness | 🟢 / 🟡 / 🔴 | [X] |

**Overall Health**: [X]% accurate

---

## 🔴 Critical Issues

### Missing Documentation

| Item | Type | Suggested Action |
|------|------|------------------|
| [service] | Docker | Add to Services doc |
| [guest] | LXC | Add to Proxmox doc |

### Incorrect or Conflicting Information

| File | Issue | Current | Should Be |
|------|-------|---------|-----------|
| IP Map | Duplicate IP | [ip] → A and B | Re-IP one host |
| CLAUDE.md | Stale version | Proxmox 8.x | Proxmox 9.x (live) |

---

## 🟡 Warnings

### Outdated Entries

| File | Last Updated | Days Stale |
|------|--------------|------------|
| [file] | [date] | [X] |

### Still Mentions Retired Components

| File | Mentions |
|------|----------|
| [file] | [Kubernetes nodes listed as active] |

### Stale Tasks

| Task | In Progress Since |
|------|-------------------|
| [task] | [date] |

---

## 🟢 Verified Accurate

- ✅ [Doc] - [What was verified]

---

## 📋 Recommended Actions

### High Priority
1. [ ] [Fix]

### Medium Priority
2. [ ] [Fix]

### Low Priority
3. [ ] [Fix]

---

## 🔧 Auto-Fix Suggestions

### Add to Services doc
```markdown
| [Service] | https://[url] | [host] | [port] | [auth] |
```

### Add to IP Map
```markdown
| [IP] | [Device] | [Type] | [Notes] |
```

### Update in `[File]`
```markdown
[Old content]
↓
[New content]
```

Would you like me to apply these fixes?
```

### Applying Fixes

When the user approves fixes, follow the **Change Documentation Protocol** in `CLAUDE.md`:
1. Edit the docs (preserve surrounding content; never delete sections without asking)
2. Bump `updated:` frontmatter on every modified doc
3. Add one Changelog entry summarizing the sync (`Changed` / `Fixed`)
4. Update the Task Registry if a doc-sync task exists

## Arguments

- `/doc-sync` - Full sync report
- `/doc-sync services` - Service catalog only
- `/doc-sync ips` - IP addresses only
- `/doc-sync guests` - VM / LXC inventory only
- `/doc-sync retired` - Find mentions of retired components
- `/doc-sync tasks` - Task Registry hygiene only
- `/doc-sync stale` - Stale content only
- `/doc-sync live` - Include live comparison via read-only commands
- `/doc-sync fix` - Generate fix suggestions
- `/doc-sync apply` - Apply fixes (with confirmation)
- `/doc-sync [filename]` - Check specific file
