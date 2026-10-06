# Troubleshoot

Search troubleshooting guides and get help with homelab issues.

## Instructions

Help diagnose and resolve homelab infrastructure issues, using your own documented history first.

### Data Sources

Read `CLAUDE.md` first, especially **Known Gotchas** and **Retired Components**. Then, using its **Documentation Structure** table, read:
- **Troubleshooting** - Known issues and fixes
- **Changelog** - Recent changes (a recent change is often the cause)
- **Services**, **Network**, **Proxmox**, **Reverse Proxy**, **SSO** - As relevant to the issue

Never open the doc listed under the **Sensitive** role.

### Approach

1. Match the symptoms against Known Gotchas and the Troubleshooting doc first.
2. Check the Changelog for changes in the last few days that touch the affected service, host, or IP.
3. Suggest read-only diagnostics before any fix. Ask before restarting or changing anything.
4. After a fix, offer to add a Troubleshooting entry and a Changelog `Fixed` entry.

### Common Issue Categories

1. **Container Issues** - Docker/LXC problems
2. **Network Issues** - Connectivity, DNS, routing, VLANs, stale ARP
3. **Proxmox Issues** - Cluster, quorum, VMs, storage
4. **Kubernetes Issues** - Pods, services, networking (if active)
5. **Service Issues** - Specific application problems
6. **Authentication Issues** - SSO, forward auth
7. **Storage Issues** - NFS, disk space, full disks from logs
8. **Certificate Issues** - TLS, Let's Encrypt

### Output Format - Issue Search

````markdown
# 🔧 Troubleshooting: [Issue Description]

## 🔍 Matching Known Issues

### Issue 1: [Title]
**Symptoms**: [What you see]
**Cause**: [Root cause]
**Solution**:
```bash
[Commands to fix]
```
**Source**: [Troubleshooting doc section / Known Gotcha]

---

## 🕑 Recent Related Changes

| Date | Change |
|------|--------|
| [date] | [Changelog entry touching this service/host] |

---

## 🔬 Diagnostic Commands

### Check Service Status
```bash
docker ps -a | grep [service]
docker logs --tail 100 [container]
systemctl status [service]
journalctl -u [service] --since "1 hour ago"
```

### Check Network
```bash
ping -c 3 [ip]
curl -v http://[ip]:[port]
dig [service].[domain]
curl -H "Host: [service].[domain]" http://[proxy-ip]
```

### Check Resources
```bash
df -h
free -h
du -sh /var/lib/docker/containers/*/*-json.log | sort -h | tail
```

---

## 🎯 Suggested Actions

1. **First**: [Quick fix attempt]
2. **If that fails**: [Next step]
3. **Escalation**: [More involved fix]

---

## ❓ Still Stuck?

Would you like me to:
1. Generate a diagnostic script?
2. Search all documentation for related issues?
3. Add this as a new troubleshooting entry?
````

### Generic Gotchas Worth Checking

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Ping works, TCP says "connection refused" right after a CT/VM was recreated or its MAC changed | Stale ARP entry on Proxmox nodes | `ip neigh flush dev vmbr0 [ip]` on each node |
| Two hosts flap on the same IP | Duplicate IP allocation | `/ip-find conflicts`, then re-IP one host |
| Disk suddenly full on a Docker host | Unrotated container logs | Set json-file `max-size`/`max-file` in `/etc/docker/daemon.json` |
| NFS mounts hang at boot | Mount without `_netdev`/`soft` | Use `soft,timeo=30,retrans=3,_netdev` |
| 404 from reverse proxy | Router rule or entrypoint mismatch | Check the router in the proxy dashboard/logs |
| Redirect loop on SSO | Forward-auth middleware on the SSO host itself | Exclude the auth domain from the middleware |
| Cluster actions fail with "no quorum" | Node or QDevice down | `pvecm status`, restore the node/QDevice |

### Quick Diagnostics by Category

**Docker**:
```bash
docker ps -a
docker logs [container]
docker inspect [container]
docker stats --no-stream
```

**Network**:
```bash
ip addr
ip route
ip neigh
ss -tulpn
```

**Proxmox**:
```bash
pvecm status
qm list
pct list
pvesm status
```

**Kubernetes**:
```bash
kubectl get nodes
kubectl get pods -A
kubectl describe pod [pod]
kubectl logs [pod]
```

### Add New Troubleshooting Entry

Append to the **Troubleshooting** doc, then add a Changelog `Fixed` entry:

````markdown
## [Issue Title]

**Symptoms**:
- [What you observe]

**Cause**:
[Root cause explanation]

**Solution**:
```bash
[Fix commands]
```

**Prevention**:
[How to prevent in future]
````

If the issue is a recurring trap, also suggest adding a one-line rule under **Known Gotchas** in `CLAUDE.md`.

## Arguments

- `/troubleshoot [description]` - Search for matching issues
- `/troubleshoot docker` - Docker-specific help
- `/troubleshoot network` - Network troubleshooting
- `/troubleshoot proxmox` - Proxmox issues
- `/troubleshoot k8s` - Kubernetes issues
- `/troubleshoot auth` - Authentication issues
- `/troubleshoot [service-name]` - Service-specific help
- `/troubleshoot add` - Add new troubleshooting entry
- `/troubleshoot diag [service]` - Generate diagnostic commands
