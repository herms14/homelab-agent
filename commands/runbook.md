# Runbook

Generate operational runbooks and procedures for homelab tasks.

## Instructions

Create step-by-step operational procedures for common homelab tasks.

### Data Sources

Read all homelab documentation for context:
- `07 HomeLab Things/Claude Managed Homelab/` - All files
- Focus on procedures already documented

### Available Runbooks

| Runbook | Purpose |
|---------|---------|
| `maintenance` | Proxmox node maintenance |
| `backup` | Backup and restore procedures |
| `disaster-recovery` | DR procedures |
| `service-restart` | Restart services safely |
| `upgrade` | System/service upgrades |
| `scale` | Add resources/nodes |
| `network` | Network changes |
| `security` | Security procedures |

### Output Format

```markdown
# 📖 Runbook: [Title]

## Overview

| Property | Value |
|----------|-------|
| **Purpose** | [What this runbook accomplishes] |
| **Duration** | [Estimated time] |
| **Risk Level** | Low / Medium / High |
| **Requires Downtime** | Yes / No / Partial |
| **Last Updated** | [Date] |
| **Author** | Homelab Agent |

---

## Prerequisites

- [ ] [Prerequisite 1]
- [ ] [Prerequisite 2]
- [ ] [Access/permissions needed]

---

## Pre-Checks

Before starting, verify:

```bash
# [Check commands]
```

- [ ] [Verification 1]
- [ ] [Verification 2]

---

## Procedure

### Step 1: [Step Title]

**Purpose**: [Why this step]

```bash
# Commands for this step
[command 1]
[command 2]
```

**Expected Output**:
```
[What you should see]
```

**If this fails**: [What to do]

---

### Step 2: [Step Title]

**Purpose**: [Why this step]

```bash
[commands]
```

---

### Step 3: [Step Title]

...

---

## Verification

After completing all steps:

```bash
# Verification commands
```

- [ ] [Verification check 1]
- [ ] [Verification check 2]
- [ ] [Service accessible]
- [ ] [No errors in logs]

---

## Rollback Procedure

If something goes wrong:

### Rollback Step 1
```bash
[rollback commands]
```

### Rollback Step 2
```bash
[rollback commands]
```

---

## Troubleshooting

### Common Issues

**Issue**: [Description]
**Solution**: [Fix]

**Issue**: [Description]
**Solution**: [Fix]

---

## Post-Procedure

- [ ] Update documentation if needed
- [ ] Log changes to changelog: `/lab-changelog add`
- [ ] Notify team/users if applicable
- [ ] Monitor for issues

---

## Related Documentation

- [[Related Doc 1]]
- [[Related Doc 2]]

---

## Change History

| Date | Change | Author |
|------|--------|--------|
| [Date] | Initial creation | Homelab Agent |
```

### Runbook Templates

**Node Maintenance**:
1. Pre-checks (cluster quorum, VM inventory)
2. Enable maintenance mode
3. Migrate VMs
4. Perform maintenance
5. Restore normal operation
6. Verify

**Backup Procedures**:
1. List what to backup
2. Verify backup targets
3. Execute backups
4. Verify backup integrity
5. Document completion

**Service Restart**:
1. Identify dependencies
2. Notify users
3. Stop service gracefully
4. Verify stopped
5. Start service
6. Verify running
7. Test functionality

**Disaster Recovery**:
1. Assess situation
2. Identify recovery point
3. Restore from backup
4. Verify data integrity
5. Test services
6. Document incident

## Arguments

- `/runbook` - List available runbooks
- `/runbook maintenance` - Node maintenance procedure
- `/runbook backup` - Backup procedures
- `/runbook disaster-recovery` or `/runbook dr` - DR procedures
- `/runbook service-restart [service]` - Restart specific service
- `/runbook upgrade [component]` - Upgrade procedure
- `/runbook scale [component]` - Scaling procedure
- `/runbook network [change-type]` - Network changes
- `/runbook security [task]` - Security procedures
- `/runbook custom [title]` - Generate custom runbook
