# 3mbyPanel v0.2.0

A standalone, multi-server Emby management control plane for Ubuntu 24.04.

3mbyPanel does not require a bundled Emby server. A fresh installation starts only the panel, PostgreSQL, Redis, Operations Agent, and secured M3U/XMLTV gateway. Register any existing Emby server from the Servers page; the first registered server becomes the primary user-management server.

## Main capabilities

- Independent management of multiple Emby servers.
- Existing Emby user import, linking, expiration, suspension, renewal, and password synchronization.
- Packages, simultaneous stream limits, resellers, sub-resellers, credits, and audit history.
- Fleet dashboard with server API health, active streams, panel CPU, memory, disk, and network information.
- Per-server Library Nodes installed remotely through SSH as Docker Compose or native Python/systemd.
- Incremental per-server STRM/NFO libraries with existing-file adoption, duplicate protection, missing-file grace, and daily schedules.
- Per-server M3U source and category assignments.
- Secured, server-specific M3U and XMLTV feed URLs.
- Background large-playlist imports with PostgreSQL bulk reconciliation and job progress.
- Independent Emby tuner, listing-provider, and guide-refresh synchronization for each server.
- Stream Operations across all servers.
- Operations Center, managed backups, restore, diagnostics, repair, upgrade, and rollback.

## Default ports

| Service | Port |
|---|---:|
| 3mbyPanel web interface | 8281 |
| Control API, localhost only | 8282 |
| Secured M3U/XMLTV gateway | 8283 |
| Optional local Emby | 8098 |
| Remote Library Node | 8392 |

Installation directory: `/opt/3mbypanel`

## Fresh standalone installation

```bash
cd /root
sha256sum -c 3mbypanel-v0.2.0.zip.sha256
unzip -o 3mbypanel-v0.2.0.zip
cd 3mbypanel-v0.2.0
sudo bash install.sh --standalone-only
```

Open `http://PANEL_SERVER_IP:8281`, then use **Servers → Add server**.

Enter the normal Emby address, such as `http://10.0.0.25:8096`. 3mbyPanel normalizes the internal API URL to Emby's `/emby` REST root. Create a dedicated Emby API key for each server.

## Optional local Emby profile

```bash
sudo bash install.sh --with-local-emby
```

The optional profile uses the official `emby/embyserver` image and publishes Emby on port 8098 by default.

## Upgrade from v0.1.0

```bash
cd /root
sha256sum -c 3mbypanel-v0.2.0.zip.sha256
unzip -o 3mbypanel-v0.2.0.zip
cd 3mbypanel-v0.2.0
sudo bash install.sh --mode upgrade
```

The upgrade preserves PostgreSQL data, users, packages, resellers, credits, server keys, M3U/XMLTV sources, EPG mappings, STRM files, backups, and audit history. Database schema advances from 15 to 19.

## Useful commands

```bash
sudo /opt/3mbypanel/scripts/manage.sh health
sudo /opt/3mbypanel/scripts/manage.sh servers
sudo /opt/3mbypanel/scripts/manage.sh libraries
sudo /opt/3mbypanel/scripts/manage.sh deployments
sudo /opt/3mbypanel/scripts/manage.sh m3u-jobs
sudo /opt/3mbypanel/scripts/manage.sh diagnostics
```

The downloadable cumulative archive and checksum are distributed with the v0.2.0 build handoff. Complete runtime testing on a disposable Ubuntu 24.04 host before production use.

No media, television service, or content is included. Use only content and sources you own or are authorized to distribute.
