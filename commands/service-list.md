# Service List

List all deployed homelab services with URLs, hosts, ports, and auth methods.

## Instructions

Generate a service catalog from the homelab documentation.

### Data Sources

Read `CLAUDE.md` first (for the domain and URL pattern). Then, using its **Documentation Structure** table, read:
- **Services** - Main catalog
- **IP Map** - Host IPs
- **Proxmox** - Which VM / LXC hosts each service
- Any additional service docs referenced from the Services doc (media stack, app configs, etc.)

Never open the doc listed under the **Sensitive** role, and never print API keys or passwords even if they appear in other docs.

### Service Categories

Organize services into these categories (skip empty ones, add your own if your docs use different groupings):
1. **Media** - Jellyfin/Plex, *arr apps, download clients
2. **Core Infrastructure** - Reverse proxy, SSO, DNS, hypervisor UI, backup server
3. **Monitoring & Observability** - Grafana, Prometheus, Uptime Kuma, tracing
4. **Automation & DevOps** - Git server, CI runners, n8n, bots, agents
5. **Utilities** - Paperless, photo management, bookmarks, finance
6. **Home Automation** - Home Assistant, IoT
7. **Dashboards** - Glance, Homepage, Homarr

### Output Format

```markdown
# 📦 Service Catalog

**Total Services**: [X]+
**Domain**: [domain]
**Last Updated**: [Date]

---

## 🎬 Media ([X] services)

| Service | URL | Host | Type | Port | Auth |
|---------|-----|------|------|------|------|
| Jellyfin | https://jellyfin.[domain] | [media-lxc] | Docker | 8096 | SSO |
| Sonarr | https://sonarr.[domain] | [media-lxc] | Docker | 8989 | SSO |
| ... | ... | ... | ... | ... | ... |

---

## 🏗️ Core Infrastructure ([X] services)

| Service | URL | Host | Type | Port | Auth |
|---------|-----|------|------|------|------|
| Traefik | https://traefik.[domain] | [proxy-lxc] | LXC | 8080 | SSO |
| Authentik | https://auth.[domain] | [sso-lxc] | LXC | 9000 | Native |
| Proxmox | https://proxmox.[domain] | [node01] | Native | 8006 | PAM |
| ... | ... | ... | ... | ... | ... |

---

## 📈 Monitoring & Observability ([X] services)

| Service | URL | Host | Type | Port | Auth |
|---------|-----|------|------|------|------|
| Grafana | https://grafana.[domain] | [utility-vm] | Docker | 3000 | SSO |
| ... | ... | ... | ... | ... | ... |

---

*(repeat for each non-empty category)*

---

## 🔗 Quick Access

- [Most-used services as markdown links]

---

## ⏸️ Stopped / Retired

| Service | Status | Notes |
|---------|--------|-------|
| [service] | 🔴 Stopped | [host stopped since ...] |

---

## 📊 Service Statistics

| Metric | Count |
|--------|-------|
| Total Services | [X]+ |
| Docker Containers | [X] |
| LXC-native | [X] |
| SSO-Protected | [X] |
| Public-Facing | [X] |
```

### Search Functionality

When searching, match against:
- Service name
- URL
- Category
- Host / backend type
- Port
- Description

## Arguments

- `/service-list` - All services
- `/service-list [category]` - One category (e.g. `media`, `core`, `monitoring`, `utilities`)
- `/service-list host [name]` - Services running on a specific host
- `/service-list search [term]` - Search by name/keyword
- `/service-list url [service]` - Get URL for specific service
- `/service-list port [number]` - Find service by port
