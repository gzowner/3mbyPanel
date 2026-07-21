<div align="center">

# 3mbyPanel

### Standalone, multi-server Emby management from one control plane

[![Version](https://img.shields.io/badge/version-0.2.0-6f7cff?style=for-the-badge)](#installation)
[![Platform](https://img.shields.io/badge/panel-Ubuntu%2024.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](#supported-environments)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](#architecture)
[![Database](https://img.shields.io/badge/PostgreSQL-19-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](#architecture)
[![Status](https://img.shields.io/badge/status-early%20access-f5a623?style=for-the-badge)](#current-status)

**Manage users, packages, resellers, libraries, Live TV, EPG, active streams, and multiple independent Emby servers without installing Emby on the panel host.**

</div>

---

## What is 3mbyPanel?

3mbyPanel is a self-hosted operations platform for Emby administrators, managed-hosting providers, organizations, and advanced home-media operators.

It turns several independent Emby servers into one manageable fleet while keeping each server's users, media paths, STRM libraries, M3U sources, XMLTV providers, health state, and playback sessions separate.

A fresh installation runs as a **standalone control plane**. It does not require a bundled Emby server and does not proxy video traffic through the panel.

### Built for operators who need more than the standard server dashboard

- Manage several Emby servers from one interface.
- Import and control existing Emby users.
- Sell or assign subscription-style packages.
- Delegate customer management to resellers.
- Apply expiration dates and simultaneous-stream limits.
- Deploy remote Library Nodes without manually building every server.
- Build independent STRM libraries for each server.
- Assign different M3U categories to different Emby servers.
- Monitor active streams, devices, IP addresses, and transcoding.
- Diagnose, back up, repair, and upgrade the platform from one place.

---

## Feature overview

| Area | Capabilities |
|---|---|
| **Multi-server control** | Register independent Emby servers, encrypted API keys, primary-server selection, API health, version checks, per-server failures, and combined fleet status. |
| **User lifecycle** | Import existing users, create and link accounts, enable, suspend, renew, expire, delete, synchronize policies, and reset passwords across assigned servers. |
| **Packages** | Connection limits, account duration, Live TV access, movie access, TV-show access, download permissions, and reseller credit cost. |
| **Resellers** | Administrator, master-reseller, reseller, sub-reseller, credit balances, credit history, customer ownership, and scoped visibility. |
| **Live TV and EPG** | Multiple M3U sources, category filtering, per-server source assignment, secured M3U/XMLTV feeds, tuner creation, listing providers, and guide refresh. |
| **Large playlists** | Background imports, job progress, duplicate-job protection, PostgreSQL bulk reconciliation, atomic replacement, and rollback to the last working playlist. |
| **STRM libraries** | Per-server media inventory, incremental STRM/NFO generation, existing-file adoption, duplicate protection, missing-file grace, and daily schedules. |
| **Remote deployment** | Install Library Nodes over SSH using Docker Compose or native Python/systemd. Password and private-key authentication are supported. |
| **Stream operations** | Active users, server, device, IP, media title, playback method, bitrate, direct play, direct stream, transcoding, messages, session stopping, and limit enforcement. |
| **Operations** | Fleet dashboard, CPU, memory, disk, network, server status, service health, controlled restarts, diagnostics, backups, restore, repair, upgrade, and rollback. |
| **Audit and safety** | Activity history, administrator protection, encrypted server credentials, redacted diagnostics, secured feed tokens, and optional automatic enforcement. |

---

## Why operators choose this model

### One panel, independent servers

Every registered Emby server remains its own system. A failure or maintenance window on one server does not merge or overwrite another server's configuration.

### No central video proxy

3mbyPanel controls Emby through its API and publishes secured M3U/XMLTV feeds. Video playback stays between the Emby server, its storage, and the client.

### Incremental automation

Large playlists and media libraries are processed as tracked jobs. Existing working data remains available until a successful replacement is ready.

### Reseller-ready account management

Packages, expirations, credits, ownership, and connection limits provide the foundation for a managed-service or customer-account workflow.

---

## Architecture

```mermaid
flowchart LR
    A[3mbyPanel Web UI] --> B[Control API]
    B --> C[(PostgreSQL)]
    B --> D[(Redis)]
    B --> E[Background Worker]
    B --> F[M3U / XMLTV Gateway]
    B --> G[Operations Agent]

    B --> H[Emby Server A]
    B --> I[Emby Server B]
    B --> J[Emby Server C]

    H --> K[Library Node A]
    I --> L[Library Node B]
    J --> M[Library Node C]

    K --> N[(Media Storage A)]
    L --> O[(Media Storage B)]
    M --> P[(Media Storage C)]
```

### Central services

- Web interface
- Control API
- Background worker
- PostgreSQL
- Redis
- Operations Agent
- Secured M3U/XMLTV gateway

### Services that stay on each media server

- Emby Server
- Media storage or rclone mount
- Transcoding
- Optional Library Node
- Generated STRM/NFO library

---

## Product tour

### Fleet dashboard

See registered servers, API health, versions, active streams, panel health, CPU, memory, free storage, network totals, and Library Node status.

### Server registry

Add any existing Emby server using its normal address and a dedicated API key. The first registered server becomes the primary account-management server.

### User management

Import existing accounts without changing their current passwords, assign packages, set expiration dates, synchronize policies, and link the same customer across several Emby servers.

### Per-server libraries

Install a Library Node remotely, scan that server's local media, adopt existing STRM files, generate only missing or changed entries, and refresh only the affected Emby library.

### Live TV and EPG

Assign specific M3U sources and categories to each Emby server. 3mbyPanel creates secured server-specific playlist and XMLTV URLs, configures the tuner and listing providers, and triggers a guide refresh.

### Stream Operations

View who is watching, where they connected from, which device they use, whether playback is direct or transcoded, and stop or message individual sessions.

---

## Installation

### Fresh standalone installation

The recommended installation does **not** install Emby on the panel host.

```bash
cd /root
sha256sum -c 3mbypanel-v0.2.0.zip.sha256
unzip -o 3mbypanel-v0.2.0.zip
cd 3mbypanel-v0.2.0
sudo bash install.sh --standalone-only
```

Open:

```text
http://PANEL_SERVER_IP:8281
```

Then select:

```text
Servers → Add server
```

### Optional local Emby profile

For a lab, demonstration, or all-in-one installation:

```bash
sudo bash install.sh --with-local-emby
```

The optional local Emby container uses port `8098` by default.

### Upgrade from v0.1.0

```bash
cd /root
sha256sum -c 3mbypanel-v0.2.0.zip.sha256
unzip -o 3mbypanel-v0.2.0.zip
cd 3mbypanel-v0.2.0
sudo bash install.sh --mode upgrade
```

The upgrade preserves users, packages, resellers, credits, API keys, server assignments, M3U/XMLTV sources, EPG mappings, STRM inventory, backups, and audit history.

---

## Default ports

| Service | Default port | Exposure |
|---|---:|---|
| 3mbyPanel web interface | `8281` | Administrator access |
| Control API | `8282` | Localhost only |
| Secured M3U/XMLTV gateway | `8283` | Registered Emby servers |
| Optional local Emby | `8098` | Emby clients |
| Remote Library Node | `8392` | Panel and local Emby host |

Installation directory:

```text
/opt/3mbypanel
```

---

## Supported environments

### Panel host

- **Recommended:** Ubuntu 24.04 LTS
- Docker Engine and Docker Compose are installed automatically when needed.
- A clean VPS or virtual machine is recommended for production testing.

### Managed Emby servers

Emby servers can run separately from the panel and only need to be reachable through the Emby API.

Remote Library Node installation is designed for Ubuntu and Debian hosts using either:

- Docker Compose
- Python virtual environment and systemd

Ubuntu 20.04 media servers may be managed through the Emby API. The central panel host remains recommended on Ubuntu 24.04, and native Library Node deployment on older distributions should be tested before production use.

---

## Recommended starting resources

For a small deployment managing several Emby servers:

| Resource | Recommended starting point |
|---|---:|
| CPU | 4 vCPU |
| Memory | 8 GB RAM |
| Storage | 100–160 GB NVMe |
| Network | 1 Gbps |
| OS | Ubuntu 24.04 LTS |

The panel does not transcode or proxy video, so media bandwidth and transcoding resources remain on the Emby servers.

---

## Remote Library Nodes

Install from the panel under:

```text
Libraries → Install node
```

Supported deployment modes:

- Docker Compose
- Native Python and systemd
- SSH password authentication
- OpenSSH private keys
- Encrypted keys with passphrases
- Root or passwordless-sudo deployment
- Optional SSH host-key verification

SSH credentials are used only during the active deployment and are not saved in PostgreSQL.

---

## Useful management commands

```bash
sudo /opt/3mbypanel/scripts/manage.sh health
sudo /opt/3mbypanel/scripts/manage.sh servers
sudo /opt/3mbypanel/scripts/manage.sh libraries
sudo /opt/3mbypanel/scripts/manage.sh deployments
sudo /opt/3mbypanel/scripts/manage.sh m3u-jobs
sudo /opt/3mbypanel/scripts/manage.sh diagnostics
```

View the generated panel administrator credentials:

```bash
sudo cat /opt/3mbypanel/panel-admin-credentials.txt
```

---

## Security model

- Remote Emby API keys are encrypted before database storage.
- Server-specific M3U/XMLTV URLs use signed feed tokens.
- Control API and database ports are not intended for public exposure.
- Diagnostic packages redact passwords, API keys, and secrets.
- SSH passwords, private keys, passphrases, and sudo passwords are not retained after a node deployment.
- Administrator accounts are protected from reseller actions.
- HTTPS or a trusted private network is strongly recommended.
- Never expose the Docker socket, PostgreSQL, Redis, or internal control ports directly to the internet.

See [`SECURITY.md`](SECURITY.md) for deployment guidance.

---

## Current status

3mbyPanel v0.2.0 is an **early-access self-hosted release**.

The package has passed validation for:

- Python and shell syntax
- Docker Compose and YAML
- Browser JavaScript and HTML references
- Database migrations through schema 19
- Emby URL and API translation
- Multi-server user assignment
- Remote Library Node deployment logic
- Per-server Live TV synchronization
- A generated 20,000-channel M3U import
- ZIP integrity and internal SHA-256 manifests

Real-world testing should be completed on a staging installation before placing the panel into production.

---

## Commercial and managed-service use

3mbyPanel is designed to provide the technical foundation for managed Emby hosting, customer-account administration, reseller workflows, hospitality systems, community media systems, and private organizational deployments.

Before selling a service, operators should provide their own:

- Hosting and support terms
- Privacy policy
- Billing system
- Customer authentication policy
- Backup and retention policy
- Incident-response process
- Emby licenses and any permissions required for their business model

3mbyPanel does not include billing, payment processing, media, television channels, or content licenses.

---

## Disclaimer

3mbyPanel is an independent project and is not affiliated with, endorsed by, or sponsored by Emby.

Emby and related marks belong to their respective owners. No media, television service, subscription service, or copyrighted content is included. Use only media and sources you own or are authorized to manage and distribute.

---

<div align="center">

**One control plane. Multiple Emby servers. Independent libraries, users, Live TV, and operations.**

</div>
