# Runbook: Replace a failed disk (Synology, RAID1/SHR)

Scope
Steps to replace a physically failed HDD in a 2‑disk RAID1 (SHR) Storage Pool on Synology DSM 7.2. Follow this runbook to minimize downtime and risk.

Preconditions
- You have physical access to the NAS.
- A compatible replacement drive (equal or larger capacity) is available.
- Backups are up-to-date (verify backup status before disruptive ops).
- You are an admin on DSM.

1) Confirm the failure
- DSM notifications or Storage Manager alert: identify which drive is flagged (slot number and model).
- Storage Manager → HHD/SSD: confirm SMART status is Failed / Critical.
- Note: do not remove any other drives.

2) Prepare
- Notify users of possible degraded performance.
- Ensure UPS is connected and charged (prevent power loss during rebuilds).
- If possible, wait for a scheduled maintenance window.

3) Physically replace the drive
- Hot‑swappable models: pull the failed drive tray (confirm the indicator light is showing failure)
- Replace with the new drive in the same bay and ensure the tray is seated
- For non-hot-swap or if unsure: power down per maintenance plan, replace drive, power on

4) Start repair (DSM should auto‑start; verify)
- Storage Manager → Storage Pool → the pool should show “Degraded” with a Repair button
- Click Action → Repair (or DSM may automatically start rebuilding)
- If manual:
  - Select Storage Pool → Manage → Repair → choose the new drive as replacement
- Monitor rebuild progress in Storage Manager (may take many hours)

5) Verify post‑repair
- Once rebuilt, Storage Pool should show “Healthy”
- Check Volume/Shared Folder accessibility
- Run Data Scrubbing (optional) or schedule a scrub soon
- Check SMART on the new disk after a short time

6) Post‑operation tasks
- Update inventory or maintenance log with drive serial, replacement date, and any notes
- Dispose or return failed drive according to data retention/security policies
- Re-check Hyper Backup and snapshot tasks scheduled for the next run to ensure no missed backups

Troubleshooting
- Rebuild fails / disk not accepted:
  - Confirm drive compatibility
  - Check system logs (Log Center) for errors
  - Try reseating the drive
  - Contact Synology support if drive is rejected or pool repair cannot proceed

Notes
- Replacement in SHR/RAID1 is straightforward with hot‑swap-capable models — the system generally manages rebuild automatically.
- Always keep offsite backups separate from the NAS in case of catastrophic events.
