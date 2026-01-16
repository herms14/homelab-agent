# Doc Sync

Verify documentation accuracy and find discrepancies.

## Instructions

Check if homelab documentation matches the actual infrastructure state.

### Data Sources

Read all homelab documentation:
- `07 HomeLab Things/Claude Managed Homelab/` - All files

### Checks to Perform

1. **Service Catalog** - Are all services documented?
2. **IP Addresses** - Are IPs correctly mapped?
3. **VM Inventory** - Do VM lists match?
4. **Configuration** - Are configs up to date?
5. **Credentials** - Are credentials current?
6. **Stale Content** - Any outdated information?

### Output Format

```markdown
# 🔄 Documentation Sync Report

**Generated**: [Date]
**Files Checked**: [X]
**Issues Found**: [X]

---

## 📊 Summary

| Category | Status | Issues |
|----------|--------|--------|
| Service Catalog | 🟢 / 🟡 / 🔴 | [X] |
| IP Addresses | 🟢 / 🟡 / 🔴 | [X] |
| VM Inventory | 🟢 / 🟡 / 🔴 | [X] |
| Configurations | 🟢 / 🟡 / 🔴 | [X] |
| Freshness | 🟢 / 🟡 / 🔴 | [X] |

**Overall Health**: [X]% accurate

---

## 🔴 Critical Issues

### Missing Documentation

| Item | Type | Suggested Action |
|------|------|------------------|
| [Service] | Docker | Add to `07 - Deployed Services.md` |
| [VM] | VM | Add to `02 - Proxmox Cluster.md` |

### Incorrect Information

| File | Issue | Current | Should Be |
|------|-------|---------|-----------|
| IP Map | Wrong IP | 192.168.20.X | 192.168.20.Y |
| Services | Wrong port | 8080 | 8443 |

---

## 🟡 Warnings

### Outdated Entries

| File | Last Updated | Days Stale | Content |
|------|--------------|------------|---------|
| 11 - Credentials | 2025-11-15 | 62 days | API keys may be rotated |
| 21 - App Configs | 2025-12-01 | 46 days | Configs may have changed |

### Potential Duplicates

| Item | Locations |
|------|-----------|
| [Service] | File A, File B |

---

## 🟢 Verified Accurate

- ✅ Network Architecture - All VLANs documented
- ✅ Proxmox Cluster - Node info current
- ✅ Kubernetes Cluster - Config matches
- ✅ Storage Architecture - Mounts documented

---

## 📋 Recommended Actions

### High Priority
1. [ ] Add [service] to Deployed Services
2. [ ] Fix IP address for [device]
3. [ ] Update credentials file

### Medium Priority
4. [ ] Review stale config files
5. [ ] Update app configurations

### Low Priority
6. [ ] Add missing descriptions
7. [ ] Standardize formatting

---

## 🔧 Auto-Fix Suggestions

The following can be auto-generated:

### Add to `07 - Deployed Services.md`
```markdown
| [Service] | https://[url] | Docker | [port] | [auth] |
```

### Add to `10 - IP Address Map.md`
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

---

## 📈 Documentation Health Trend

| Date | Accuracy | Issues |
|------|----------|--------|
| Today | [X]% | [X] |
| Last Week | [X]% | [X] |
| Last Month | [X]% | [X] |

---

## 🔍 Detailed File Analysis

### Recently Modified (Good)
| File | Modified | Status |
|------|----------|--------|
| 07 - Deployed Services | Today | 🟢 Fresh |
| 02 - Proxmox Cluster | 2 days ago | 🟢 Fresh |

### Needs Review (30+ days)
| File | Last Modified | Days |
|------|---------------|------|
| 11 - Credentials | 2025-11-15 | 62 |
| 21 - App Configs | 2025-12-01 | 46 |

### Very Stale (90+ days)
| File | Last Modified | Days |
|------|---------------|------|
| [File] | [Date] | [X] |
```

### Sync Checks Detail

**Service Check**:
- List services in `07 - Deployed Services.md`
- Cross-reference with Docker configs
- Flag missing or extra entries

**IP Check**:
- Parse `10 - IP Address Map.md`
- Look for duplicate IPs
- Check for gaps in documentation

**Config Check**:
- Compare documented configs with referenced files
- Flag outdated versions
- Note missing config sections

**Freshness Check**:
- Check file modification dates
- Flag files older than 30/60/90 days
- Prioritize frequently-changing docs

## Arguments

- `/doc-sync` - Full sync report
- `/doc-sync services` - Service catalog only
- `/doc-sync ips` - IP addresses only
- `/doc-sync configs` - Configurations only
- `/doc-sync stale` - Stale content only
- `/doc-sync fix` - Generate fix suggestions
- `/doc-sync apply` - Apply auto-fixes (with confirmation)
- `/doc-sync [filename]` - Check specific file
