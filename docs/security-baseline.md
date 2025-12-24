# Security baseline — Defense‑in‑Depth for the Synology NAS

This document describes the defense‑in‑depth controls and operational posture for the photography NAS.

Principles
- Least privilege for accounts and services
- Multiple independent controls: redundancy (RAID), integrity (Btrfs checksums & scrub), point-in-time recovery (snapshots), and offsite encrypted backups
- Minimize remote exposure; prefer VPN or QuickConnect with strong controls
- Auditability and recoverability: runbooks, logs, and tested restores

[...]

Share links & client delivery hardening
- When creating shared/client links for deliverables:
  - Scope the link to the smallest possible folder (typically the job's Exports/ folder) — do not create links to parent or root (RAW) Photography folders.
  - Enable password protection on the shared link and store the password securely (password manager).
  - Set an expiration date so the link auto‑expires after client delivery.
  - Disable upload/edit options on the shared link so the recipient can download files only.
  - Audit and record each link creation (who created it, when, which job, expiration time).
  - Revoke links early if a compromise or accidental sharing is suspected.
- Prefer delivering final assets via an expiring, password‑protected share link rather than creating a publicly browsable share.

[...] (rest of file unchanged)
