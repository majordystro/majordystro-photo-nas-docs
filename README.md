# Synology NAS — Photography Workflow & Security (Documentation)

Last updated: 2025-12-24

Project overview
This repository documents and standardizes a Synology NAS–based workflow for professional photography (Capture One). It prioritizes repeatability, operational runbooks, and a defense‑in‑depth security posture.

Note on applicability
While this repository and examples are written with Capture One in mind, the same high‑level logic and workflow translate directly to other editing software (Photoshop, Lightroom, and modern AI image‑editing tools). The steps — ingest to NAS, work on files (locally or on NAS), export to a project Exports/ folder, and protect/deliver via snapshot + backup — remain the same across most editors. Adjust cache and performance settings per application as needed.

Primary goals
- Define a clear, repeatable Capture One workflow using the NAS as primary storage (not for live tethering).
- Implement defense‑in‑depth controls: RAID (SHR/RAID1), Btrfs checksums & snapshots, access controls, remote‑access hardening, and encrypted offsite backups.
- Provide runbooks/SOPs for common operations: disk replacement, snapshot & backup restores, and remote access hardening.
- Keep all design, configuration, and procedures version‑controlled in this repository.

Environment summary (scaffolding)
- NAS OS: Synology DSM 7.2
- Storage pool: 2× HDD in RAID‑1 (SHR / mirror)
- File system: Btrfs on Volume 1 (checksums, data scrubbing, snapshots)
- SSD: 1× NVMe for read/write SSD cache (write‑back)
- Protocol: SMB primary for macOS/Windows clients (Capture One)
- Backups (planned): Hyper Backup → local USB; Hyper Backup → S3‑compatible (Backblaze B2); encrypted backup vaults

Repository layout (this repo)
- README.md (this file)
- docs/
  - architecture.md
  - security-baseline.md
  - workflows/capture-one.md
  - runbooks/
    - replace-failed-disk.md
    - restore-from-snapshot.md
    - restore-from-backup.md
    - remote-access-hardening-checklist.md
- diagrams/ (placeholder for PNG/SVG diagrams)

How to use this repo
- Keep this repo authoritative for configuration and procedures.
- Update runbooks after any real-world operation and document lessons learned.
- Store no secrets (no hostnames, IDs, API keys) in this repo. Use a secrets manager or encrypted store for credentials and QuickConnect IDs if needed.
