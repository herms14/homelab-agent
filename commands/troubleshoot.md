# Troubleshoot

Search troubleshooting guides and get help with homelab issues.

## Instructions

Help diagnose and resolve homelab infrastructure issues.

### Data Sources

Read these files:
- `07 HomeLab Things/Claude Managed Homelab/12 - Troubleshooting.md` - Known issues
- `07 HomeLab Things/Claude Managed Homelab/25 - Homelab Master Wiki.md` - Reference
- All other homelab docs for context

### Common Issue Categories

1. **Container Issues** - Docker/LXC problems
2. **Network Issues** - Connectivity, DNS, routing
3. **Proxmox Issues** - Cluster, VMs, storage
4. **Kubernetes Issues** - Pods, services, networking
5. **Service Issues** - Specific application problems
6. **Authentication Issues** - Authentik, SSO
7. **Storage Issues** - NFS, disk space
8. **Certificate Issues** - SSL/TLS, Let's Encrypt

### Output Format - Issue Search

```markdown
# 🔧 Troubleshooting: [Issue Description]

## 🔍 Matching Known Issues

### Issue 1: [Title]
**Symptoms**: [What you see]
**Cause**: [Root cause]
**Solution**:
```bash
[Commands to fix]
```
**Related Docs**: [[12 - Troubleshooting#Section]]

---

### Issue 2: [Title]
**Symptoms**: [What you see]
**Cause**: [Root cause]
**Solution**: [Steps]
**Related Docs**: [[Doc Name]]

---

## 🔬 Diagnostic Commands

### Check Service Status
```bash
# Docker
docker ps -a | grep [service]
docker logs --tail 100 [container]

# Systemd
systemctl status [service]
journalctl -u [service] --since "1 hour ago"
```

### Check Network
```bash
# Test connectivity
ping [ip]
curl -v http://[ip]:[port]

# DNS resolution
nslookup [domain]
dig [domain]

# Check Traefik routing
curl -H "Host: [service].hrmsmrflrii.xyz" http://localhost:8080
```

### Check Resources
```bash
# Disk space
df -h

# Memory
free -h

# CPU/processes
htop
```

### Check Logs
```bash
# Traefik
docker logs traefik --tail 100 | grep [service]

# Authentik
docker logs authentik-server --tail 100

# Proxmox
journalctl -u pvedaemon --since "1 hour ago"
```

---

## 🎯 Suggested Actions

Based on your issue, try:

1. **First**: [Quick fix attempt]
2. **If that fails**: [Next step]
3. **Escalation**: [More involved fix]

---

## 📚 Related Documentation

- [[12 - Troubleshooting]] - Full troubleshooting guide
- [[09 - Traefik Reverse Proxy]] - Routing issues
- [[14 - Authentik Google SSO Setup]] - Auth issues
- [[02 - Proxmox Cluster]] - Cluster issues

---

## ❓ Still Stuck?

Would you like me to:
1. Generate a detailed diagnostic script?
2. Search all documentation for related issues?
3. Help create a new troubleshooting entry?
4. Check service-specific documentation?
```

### Quick Diagnostics by Category

**Docker Issues**:
```bash
docker ps -a                    # List all containers
docker logs [container]         # Check logs
docker inspect [container]      # Full details
docker stats                    # Resource usage
```

**Network Issues**:
```bash
ip addr                         # Network interfaces
ip route                        # Routing table
ss -tulpn                       # Listening ports
ping/curl/traceroute           # Connectivity
```

**Proxmox Issues**:
```bash
pvecm status                    # Cluster status
qm list                         # VM list
pct list                        # Container list
pvesm status                    # Storage status
```

**Kubernetes Issues**:
```bash
kubectl get nodes               # Node status
kubectl get pods -A             # All pods
kubectl describe pod [pod]      # Pod details
kubectl logs [pod]              # Pod logs
```

### Add New Troubleshooting Entry

When adding a new issue to documentation:

```markdown
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

**Related**: [[Doc Links]]
```

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
