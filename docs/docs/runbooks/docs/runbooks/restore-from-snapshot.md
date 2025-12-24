# Runbook: Restore files from a Btrfs snapshot (Synology DSM)

Scope
Restore an accidentally deleted or changed file/folder from a Shared Folder snapshot on DSM 7.2.

Preconditions
- Snapshots are enabled and running for the /Photography shared folder.
- You have an account with permission to restore or an admin account.

Option A — DSM GUI (recommended for most users)
1) Open "Control Panel" → "Shared Folder"
2) Select the shared folder (e.g., Photography) → "Snapshot" tab (or use Snapshot Replication app if used)
3) Click "Snapshot List" (or "Manage Snapshots") and find the snapshot corresponding to the date/time you want to restore from
4) Click "Browse" to open the snapshot contents in File Station
5) In File Station, navigate to the file(s)/folder(s) needed
6) Select file(s) and choose "Copy to" (or "Restore") and target the original location (or a temporary recovery location for verification)
7) Verify restored files and permissions
8) If restoring many files, consider restoring to a temporary path and then running an `rsync` from the temporary path into the production path to avoid partial state exposure

Option B — Snapshot Replication package (if using snapshots + replication)
- Use Snapshot Replication → Protect → Browse snapshots → Restore or Export as needed

Option C — CLI (advanced; use if GUI unavailable)
- SSH into NAS (only if allowed and with admin privileges)
- Use btrfs tooling directly with caution; Synology’s management overlays may not expose raw btrfs snapshots in typical locations
- Prefer GUI unless you have Synology CLI snapshot experience

Validation
- After restore, verify:
  - File integrity (open or checksum)
  - Correct owner/group and permissions
  - No orphaned temporary files remain

Rollbacks and clones
- If you need a full dataset rollback, consider cloning the snapshot to a new shared folder and validating before switching production paths
- Snapshot clone approach:
  - Create a temporary folder from the snapshot
  - Verify contents and ownership
  - Swap or copy data back into production once validated

Notes & cautions
- Snapshots are not backups — they can protect against accidental changes but won't protect from pool-level failure or destructive operations that propagate to snapshots (rare).
- Always keep backups (Hyper Backup) for long-term recovery and catastrophic events.
