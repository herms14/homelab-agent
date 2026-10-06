# Lab Changelog

Log infrastructure changes to maintain an audit trail, and keep the Task Registry in step.

## Instructions

Manage the homelab infrastructure changelog. This command implements step 1 of the **Change Documentation Protocol** in `CLAUDE.md`, and reminds you of steps 2-4.

### Changelog Location

Use the doc listed under the **Changelog** role in `CLAUDE.md`. If none is configured, offer to create `Homelab Changelog.md` in the docs root from `templates/Homelab Changelog.md`.

Newest entries go at the top, under a `## [YYYY-MM-DD] - Short title` heading. If today's heading already exists, append to it instead of creating a second one.

### Change Categories

- **Added** - New VMs, containers, services, configurations
- **Changed** - Modifications, updates, reconfigurations
- **Fixed** - Bug fixes, issue resolutions
- **Removed** - Decommissioned services, deleted resources
- **Infrastructure** - Hardware, network, cluster changes
- **Security** - Security updates, access changes, key rotations (never log the secret itself)

### Output Format - View Changelog

```markdown
# 📋 Homelab Changelog

> Infrastructure change log for [Cluster Name]

---

## [YYYY-MM-DD] - [Brief Description]

### Added
- ✅ New service: [Name] - [Description]
  - File or resource affected: `path/to/file`

### Changed
- 🔄 Increased [VM] memory: [old] → [new]

### Fixed
- 🔧 Resolved [issue description]

### Infrastructure
- 🖥️ Added [hardware/component]

### Security
- 🔒 Rotated [service] API key

---

## [YYYY-MM-DD] - Previous Entry

...
```

### Output Format - Add Entry

```markdown
# 📋 Add Changelog Entry

**Date**: [Current Date]
**Category**: [Added/Changed/Fixed/Removed/Infrastructure/Security]
**Description**: [Your input]

## Entry Preview

### [Category]
- [emoji] [Description]
  - File or resource affected: `[path or resource]`

---

✅ Entry added to Homelab Changelog

## Protocol Follow-ups
- [ ] Relevant doc updated? → [suggested doc role, e.g. Services / IP Map / Proxmox]
- [ ] `updated:` frontmatter bumped on modified docs
- [ ] Task Registry updated → [matching task, if found]
```

When a matching task exists in the **Task Registry**, offer to mark it `✅ Completed` with today's timestamp and the changelog line as the note.

### Auto-Detection

When run without a description, look for undocumented changes:

1. **Modified docs** in the homelab docs folder (last 24h)
2. **Git commits** if the docs folder is a repo (`git log --since="24 hours ago" --oneline`)
3. **Task Registry** items marked `✅ Completed` today with no Changelog entry
4. **Prompt** the user to describe anything else

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

When removing something significant (a cluster, a host, a whole stack), also suggest adding it to **Retired Components** in `CLAUDE.md` so other commands stop reporting it.

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

```markdown
## Monthly Summary - [Month YYYY]

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
- `/lab-changelog detect` - Find undocumented changes (auto-detection only)
- `/lab-changelog export` - Export as markdown
