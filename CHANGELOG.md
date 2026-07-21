# 3mbyPanel Changelog

## 0.2.0 — Standalone Multi-Server Parity

- Rebuilt the panel as a standalone control plane; local Emby is optional.
- First registered external Emby server becomes primary.
- Added fleet dashboard with health, CPU, memory, disk, network, versions, streams, and Library Node state.
- Added remote Library Node deployment through SSH using Docker Compose or native Python/systemd.
- Added independent per-server STRM library inventory, adoption, verification, schedules, missing-file grace, and Emby refresh.
- Added per-server M3U source and category assignments.
- Added secured server-specific M3U/XMLTV feed tokens.
- Added background large-playlist refresh jobs and PostgreSQL bulk channel reconciliation.
- Preserves the previous playlist snapshot until a refresh commits successfully.
- Added per-server Emby M3U tuner and XMLTV listing-provider synchronization.
- Added independent guide refresh for each Emby server.
- Added fleet-registry resilience and browser request timeouts.
- Added safe JSON-list normalization for M3U server assignments.
- Added existing-user import preview and multi-server user linking.
- Added optional managed-local Emby backup using a brief quiesced filesystem archive.
- Database schema advances from 15 to 19.

## 0.1.0 — Initial Emby Port

- Initial Emby-native user, package, reseller, M3U/XMLTV, EPG, STRM mirror, session, operations, backup, multi-server, and existing-user import release.
