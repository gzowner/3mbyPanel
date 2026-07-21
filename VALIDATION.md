# 3mbyPanel v0.2.0 Validation

## Completed build checks

- Parsed and byte-compiled 33 Python files.
- Validated shell syntax for 31 installer, upgrade, repair, diagnostics, management, backup, and Library Node scripts.
- Parsed the main Compose file, Intel/AMD GPU override, and remote Library Node Compose file.
- Validated the complete browser JavaScript bundle with Node.js.
- Verified 414 unique HTML identifiers and 393 JavaScript element references with no duplicate or missing IDs.
- Verified the database migration sequence is complete from schema 2 through schema 19.
- Preserved the original v0.1.0 migrations through schema 15 and added schemas 16–19 for per-server libraries, remote node deployment, standalone/per-server Live TV, and large M3U jobs.
- Tested Emby URL normalization for plain server URLs, `/emby` API URLs, and `/web` browser URLs.
- Confirmed Emby API compatibility translations for `Users/Query`, user deletion, password updates, `X-Emby-Token`, and `SimultaneousStreamLimit`.
- Parsed a generated 20,000-channel M3U containing 50 categories and one XMLTV URL.
- Verified the large-playlist code uses PostgreSQL bulk-copy staging and atomic channel reconciliation.
- Verified secured server-specific M3U/XMLTV routes and HMAC feed validation.
- Verified the per-server Emby Live TV workflow contains tuner-host, listing-provider, saved-connection, and guide-refresh operations.
- Verified the standalone Control API has no required dependency on the optional local Emby service.
- Verified the optional local Emby service uses the official `emby/embyserver` image and the `managed-emby` Compose profile.
- Verified no JellyPanel/GalaxyTV branding or Jellyfin-specific ports remain in the release source.
- Verified ZIP CRC integrity, Unix executable metadata, clean extraction, and 121 internal SHA-256 checksums.

## Runtime limitation

Docker, PostgreSQL, Redis, and a real Emby server were unavailable in the build environment. Complete the included test plans on a disposable Ubuntu 24.04 installation before production use.
