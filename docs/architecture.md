# Architecture — Synology NAS Photography Storage

Purpose
Document the deployed NAS architecture, storage layout, and sharing model so the environment is reproducible and auditable.

Hardware / DSM
- Appliance: Synology DiskStation (example family: DS920+/DS1522+ or similar)
- DSM: DiskStation Manager 7.2
- Drives:
  - 2 × HDD in a single Storage Pool as RAID‑1 (SHR-style mirror)
  - 1 × NVMe SSD configured as read/write SSD cache (write‑back) for Volume 1
- UPS: recommend external UPS + NUT integration for graceful shutdown

Storage pool and volumes
- Storage Pool: Storage Pool 1 — RAID1 (2 drives)
- Volume: Volume 1 on Storage Pool 1 formatted as Btrfs
  - Btrfs features used:
    - Checksums for metadata and data
    - Local shared folder snapshots
    - Scheduled data scrubbing
- SSD cache:
  - NVMe used as read/write cache (write‑back) to accelerate random I/O
  - Monitor cache health & hit ratio; configure write cache flush policy consistent with UPS protection

Shares and folders (example)
- Shared folder: Photography (path: /volume1/Photography)
  - Per-job layout:
    - /Photography/Client Name - YYYY-MM-DD/
      - RAWs/
      - Selects/
      - Exports/
      - Retouch/
      - CaptureOne/ (created by Capture One app)
- DSM user/group model:
  - Group: photographers
  - Users: photographer, assistant
  - Share permissions: grant photographers group read/write on /Photography; enforce quota if desired

SMB (file sharing)
- Primary protocol: SMB (Windows & macOS clients)
- DSM path: Control Panel → File Services → SMB/AFP/NFS → SMB enabled
- Recommended SMB settings:
  - Minimum SMB protocol: SMB2 (disable SMB1)
  - Disable NTLMv1, prefer NTLMv2
  - (Optional) Enable SMB signing for additional integrity on sensitive networks
  - Limit maximum concurrent connections per client if needed
- Client behavior:
  - Capture One opens projects via SMB paths; local tethering remains on laptop (external SSD) for performance.

Snapshots & data integrity
- Snapshots: enable Shared Folder snapshots for /Photography
  - Snapshot cadence (example):
    - Hourly or 2‑hourly snapshots for recent activity (keep 5 days of hourly/2‑hourly)
    - Daily snapshots retained for 30 days
    - Weekly or monthly long-term snapshots are retained longer if storage allows
- Data scrubbing (Btrfs):
  - Schedule data scrubbing weekly or biweekly using Storage Manager → Data Scrubbing
- SMART / drive health:
  - Enable SMART tests and configure automatic email alerts on failure

Backups & offsite
- Local backup: Hyper Backup → local USB HDD (rotated)
- Offsite cloud: Hyper Backup → S3‑compatible (Backblaze B2 or similar) with client-side encryption enabled (Hyper Backup vault encryption)
- Backup strategy goals: 3‑2‑1 (Primary NAS, local backup, offsite encrypted backup)
- Backup frequency: nightly incremental to cloud; hourly/daily snapshots still provide quick recovery points

Remote access
- QuickConnect: enabled for selective services only; restrict accessible services
- Avoid direct port forwarding for sensitive services; prefer VPN (WireGuard/OpenVPN) for admin tasks in the future
- Remote management: restrict DSM login accounts that can access via QuickConnect

Monitoring & logging
- DSM notifications: configure email/SMS/push notifications for critical events
- Audit logs: keep login and admin activity logs and periodically review
- Optional advanced monitoring: deploy a small VM or service for Prometheus/node_exporter and collect metrics from NAS if supported

Capacity planning & limits
- With 2×8 TB mirrored, expect ~8 TB usable; plan retention and archival policy
- Set per‑share quotas if multi‑project concurrency becomes a problem

Notes & constraints
- RAID is redundant, not a substitute for backups.
- Btrfs provides snapshots and checksumming; pair with regular backups to satisfy RTO/RPO.
