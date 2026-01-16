# Lab Changelog

Log infrastructure changes to maintain an audit trail.

## Instructions

Manage the homelab infrastructure changelog for tracking all changes.

### Changelog Location

Create/update: `07 HomeLab Things/Claude Managed Homelab/Homelab Changelog.md`

### Change Categories

- **Added** - New VMs, containers, services, configurations
- **Changed** - Modifications, updates, reconfigurations
- **Fixed** - Bug fixes, issue resolutions
- **Removed** - Decommissioned services, deleted resources
- **Infrastructure** - Hardware, network, cluster changes
- **Security** - Security updates, access changes

### Output Format - View Changelog

```markdown
# 📋 Homelab Changelog

> Infrastructure change log for MorpheusCluster

---

## [2026-01-16] - [Brief Description]

### Added
- ✅ New service: [Name] - [Description]
- ✅ New VM: [Name] on [node] - [Purpose]

### Changed
- 🔄 Updated [service] from [old] to [new]
- 🔄 Increased [VM] memory: [old] → [new]
- 🔄 Modified Traefik routing for [service]

### Fixed
- 🔧 Resolved [issue description]
- 🔧 Fixed [service] connectivity issue

### Infrastructure
- 🖥️ Added [hardware/component]
- 📦 Deployed new storage pool

### Security
- 🔒 Updated SSL certificates
- 🔒 Modified firewall rules for [purpose]

---

## [2026-01-15] - Previous Entry

...
```

### Output Format - Add Entry

When adding a new entry:

```markdown
# 📋 Add Changelog Entry

**Date**: [Current Date]
**Category**: [Added/Changed/Fixed/Removed/Infrastructure/Security]
**Description**: [Your input]

## Entry Preview

### [Category]
- [emoji] [Description]

---

✅ Entry added to Homelab Changelog

Would you like to:
1. Add another entry?
2. View full changelog?
3. Update related documentation?
```

### Auto-Detection

When run without description, check for recent changes:

1. **New files** in homelab folder (last 24h)
2. **Modified files** in homelab folder (last 24h)
3. **Git commits** if repo exists
4. **Prompt** user to describe changes

### Entry Format by Category

**Added**:
```markdown
- ✅ New [type]: [Name] - [Brief description]
  - Location: [path/host]
  - Purpose: [why added]
```

**Changed**:
```markdown
- 🔄 [What changed]: [old value] → [new value]
  - Reason: [why changed]
  - Impact: [what's affected]
```

**Fixed**:
```markdown
- 🔧 [Issue]: [Description of fix]
  - Cause: [root cause]
  - Resolution: [what was done]
```

**Removed**:
```markdown
- ❌ Removed [type]: [Name]
  - Reason: [why removed]
  - Replacement: [if any]
```

**Infrastructure**:
```markdown
- 🖥️ [Component]: [Change description]
  - Impact: [what's affected]
```

**Security**:
```markdown
- 🔒 [Security change]: [Description]
  - Scope: [what's affected]
```

### Statistics

Track monthly/weekly stats:
```markdown
## Monthly Summary - January 2026

| Category | Count |
|----------|-------|
| Added | [X] |
| Changed | [X] |
| Fixed | [X] |
| Removed | [X] |
| Infrastructure | [X] |
| Security | [X] |
| **Total** | **[X]** |
```

## Arguments

- `/lab-changelog` - View recent entries
- `/lab-changelog add "Description"` - Add new entry
- `/lab-changelog add` - Interactive add
- `/lab-changelog today` - Today's changes
- `/lab-changelog week` - This week's changes
- `/lab-changelog month` - This month's changes
- `/lab-changelog search [term]` - Search changelog
- `/lab-changelog stats` - Show statistics
- `/lab-changelog export` - Export as markdown
