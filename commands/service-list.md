# Service List

List all deployed homelab services with URLs, status, and details.

## Instructions

Generate a comprehensive service catalog from the homelab documentation.

### Data Sources

Read these files:
- `07 HomeLab Things/Claude Managed Homelab/07 - Deployed Services.md` - Main catalog
- `07 HomeLab Things/Claude Managed Homelab/08 - Arr Media Stack.md` - Media services
- `07 HomeLab Things/Claude Managed Homelab/21 - Application Configurations.md` - Configs

### Service Categories

Organize services into these categories:
1. **Media Stack** - Jellyfin, *arr apps, download clients
2. **Core Infrastructure** - Traefik, Authentik, Proxmox, GitLab
3. **Monitoring** - Grafana, Prometheus, Uptime Kuma, Jaeger
4. **Utilities** - Paperless, n8n, Immich, etc.
5. **Development** - GitLab, APIs, dev tools
6. **Home Automation** - Home Assistant, IoT
7. **Dashboards** - Glance, Homarr

### Output Format

```markdown
# 📦 Service Catalog

**Total Services**: [X]+
**Domain**: hrmsmrflrii.xyz
**Last Updated**: [Date]

---

## 🎬 Media Stack ([X] services)

| Service | URL | Backend | Port | Auth |
|---------|-----|---------|------|------|
| Jellyfin | https://jellyfin.hrmsmrflrii.xyz | Docker | 8096 | Authentik |
| Radarr | https://radarr.hrmsmrflrii.xyz | Docker | 7878 | Authentik |
| Sonarr | https://sonarr.hrmsmrflrii.xyz | Docker | 8989 | Authentik |
| Lidarr | https://lidarr.hrmsmrflrii.xyz | Docker | 8686 | Authentik |
| Prowlarr | https://prowlarr.hrmsmrflrii.xyz | Docker | 9696 | Authentik |
| Bazarr | https://bazarr.hrmsmrflrii.xyz | Docker | 6767 | Authentik |
| Overseerr | https://overseerr.hrmsmrflrii.xyz | Docker | 5055 | Native |
| Tdarr | https://tdarr.hrmsmrflrii.xyz | Docker | 8265 | Authentik |
| Deluge | https://deluge.hrmsmrflrii.xyz | Docker | 8112 | Native |
| SABnzbd | https://sabnzbd.hrmsmrflrii.xyz | Docker | 8080 | Authentik |
| ... | ... | ... | ... | ... |

---

## 🏗️ Core Infrastructure ([X] services)

| Service | URL | Backend | Port | Auth |
|---------|-----|---------|------|------|
| Traefik | https://traefik.hrmsmrflrii.xyz | LXC | 8080 | Authentik |
| Authentik | https://auth.hrmsmrflrii.xyz | LXC | 9000 | Native |
| Proxmox | https://proxmox.hrmsmrflrii.xyz | Native | 8006 | PAM |
| GitLab | https://gitlab.hrmsmrflrii.xyz | Docker | 80 | Authentik |
| Pi-hole | https://pihole.hrmsmrflrii.xyz | Docker | 80 | Native |
| ... | ... | ... | ... | ... |

---

## 📈 Monitoring ([X] services)

| Service | URL | Backend | Port | Auth |
|---------|-----|---------|------|------|
| Grafana | https://grafana.hrmsmrflrii.xyz | Docker | 3000 | Authentik |
| Prometheus | https://prometheus.hrmsmrflrii.xyz | Docker | 9090 | Authentik |
| Uptime Kuma | https://uptime.hrmsmrflrii.xyz | Docker | 3001 | Native |
| Jaeger | https://jaeger.hrmsmrflrii.xyz | Docker | 16686 | Authentik |
| ... | ... | ... | ... | ... |

---

## 🛠️ Utilities ([X] services)

| Service | URL | Backend | Port | Auth |
|---------|-----|---------|------|------|
| Paperless-ngx | https://paperless.hrmsmrflrii.xyz | Docker | 8000 | Authentik |
| Immich | https://immich.hrmsmrflrii.xyz | Docker | 2283 | Native |
| n8n | https://n8n.hrmsmrflrii.xyz | Docker | 5678 | Authentik |
| Glance | https://glance.hrmsmrflrii.xyz | LXC | 8080 | None |
| ... | ... | ... | ... | ... |

---

## 🔗 Quick Access

### Most Used
- 🎬 [Jellyfin](https://jellyfin.hrmsmrflrii.xyz) - Media streaming
- 📊 [Grafana](https://grafana.hrmsmrflrii.xyz) - Dashboards
- 🔐 [Authentik](https://auth.hrmsmrflrii.xyz) - SSO portal
- 📋 [Glance](https://glance.hrmsmrflrii.xyz) - Dashboard

### Management
- 🖥️ [Proxmox](https://proxmox.hrmsmrflrii.xyz) - Hypervisor
- 🔀 [Traefik](https://traefik.hrmsmrflrii.xyz) - Reverse proxy
- 📦 [GitLab](https://gitlab.hrmsmrflrii.xyz) - DevOps

---

## 📊 Service Statistics

| Metric | Count |
|--------|-------|
| Total Services | [X]+ |
| Docker Containers | [X] |
| LXC Containers | [X] |
| Authentik-Protected | [X] |
| Public-Facing | [X] |
```

### Search Functionality

When searching, match against:
- Service name
- URL
- Category
- Backend type
- Description

## Arguments

- `/service-list` - All services
- `/service-list media` - Media stack only
- `/service-list core` - Core infrastructure only
- `/service-list monitoring` - Monitoring only
- `/service-list utilities` - Utilities only
- `/service-list search [term]` - Search by name/keyword
- `/service-list url [service]` - Get URL for specific service
- `/service-list port [number]` - Find service by port
