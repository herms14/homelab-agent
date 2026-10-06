# Runbook

Generate operational runbooks and procedures for homelab tasks.

## Instructions

Create step-by-step operational procedures for common homelab tasks, grounded in your own documentation.

### Data Sources

Read `CLAUDE.md` first (node names, storage, backup setup, SSH user, **Known Gotchas**). Then, using its **Documentation Structure** table, read the docs relevant to the runbook (**Proxmox**, **Storage**, **Network**, **Services**, **Automation**, **Troubleshooting**) and any existing runbooks under the **Runbooks** role. Reuse procedures already documented rather than inventing new ones.

Never open the doc listed under the **Sensitive** role. Reference credentials by name only (e.g. "the PBS admin password").

### Available Runbooks

| Runbook | Purpose |
|---------|---------|
| `maintenance` | Proxmox node maintenance (migrate, update, reboot) |
| `backup` | Backup and restore procedures |
| `disaster-recovery` | DR / rebuild procedures |
| `service-restart` | Restart services safely |
| `upgrade` | System/service upgrades (incl. Proxmox major versions) |
| `patching` | Routine OS / container patching across hosts |
| `scale` | Add resources/nodes |
| `network` | Network changes (VLANs, re-IP a host) |
| `decommission` | Retire a service, host, or cluster cleanly |
| `security` | Security procedures (key rotation, access review) |

### Output Format

````markdown
# 📖 Runbook: [Title]

## Overview

| Property | Value |
|----------|-------|
| **Purpose** | [What this runbook accomplishes] |
| **Duration** | [Estimated time] |
| **Risk Level** | Low / Medium / High |
| **Requires Downtime** | Yes / No / Partial |
| **Affected** | [Hosts / services] |
| **Last Updated** | [Date] |
| **Author** | Homelab Agent |

---

## Prerequisites

- [ ] Recent backup verified for affected guests
- [ ] Task Registry checked; nobody else is working on these hosts
- [ ] [Access/permissions needed]

---

## Pre-Checks

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

...

---

## Verification

```bash
# Verification commands
```

- [ ] [Service accessible]
- [ ] [No errors in logs]

---

## Rollback Procedure

```bash
[rollback commands]
```

---

## Troubleshooting

**Issue**: [Description]
**Solution**: [Fix]

---

## Post-Procedure (Change Documentation Protocol)

- [ ] Update the relevant docs (Proxmox / Services / IP Map / Network)
- [ ] Log changes: `/lab-changelog add "..."`
- [ ] Mark the task `✅ Completed` in the Task Registry
- [ ] Monitor for issues

---

## Change History

| Date | Change | Author |
|------|--------|--------|
| [Date] | Initial creation | Homelab Agent |
````

### Runbook Templates

**Node Maintenance**:
1. Pre-checks (`pvecm status`, guest inventory, backups)
2. Migrate or shut down guests (local-storage LXCs cannot live-migrate)
3. Perform maintenance (`apt update && apt full-upgrade`)
4. Reboot, confirm quorum
5. Flush stale ARP for any re-created guests (see Known Gotchas)
6. Restore guests, verify

**Patching**:
1. Inventory hosts and exclusions (e.g. backup server, automation controller)
2. Confirm backups / snapshots
3. Patch in a maintenance window, one host at a time
4. Reboot if required
5. Verify services; log results

**Backup Procedures**:
1. List what to back up
2. Verify backup targets and free space
3. Execute backups
4. Verify integrity (test restore of one guest)
5. Document completion

**Service Restart**:
1. Identify dependencies
2. Stop service gracefully
3. Start service
4. Verify running and reachable through the reverse proxy

**Decommission**:
1. Confirm nothing depends on it (Services, reverse proxy, DNS, monitoring)
2. Final backup
3. Stop, then destroy (after confirmation)
4. Remove routing, SSO app, DNS, monitors
5. Free the IP in the IP Map; add to **Retired Components** in `CLAUDE.md`
6. Changelog `Removed` entry

**Disaster Recovery**:
1. Assess situation
2. Identify recovery point
3. Restore from backup
4. Verify data integrity
5. Test services
6. Document incident

### Saving

Offer to save the runbook to the **Runbooks** location from `CLAUDE.md` (e.g. `runbooks/[name].md`) and add a Changelog `Added` entry.

## Arguments

- `/runbook` - List available runbooks
- `/runbook maintenance [node]` - Node maintenance procedure
- `/runbook backup` - Backup procedures
- `/runbook disaster-recovery` or `/runbook dr` - DR procedures
- `/runbook service-restart [service]` - Restart specific service
- `/runbook upgrade [component]` - Upgrade procedure
- `/runbook patching` - Routine patching procedure
- `/runbook scale [component]` - Scaling procedure
- `/runbook network [change-type]` - Network changes
- `/runbook decommission [target]` - Retire a service or host
- `/runbook security [task]` - Security procedures
- `/runbook custom [title]` - Generate custom runbook
