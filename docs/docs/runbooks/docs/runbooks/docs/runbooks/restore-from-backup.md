# Runbook: Restore data from Hyper Backup (local USB / cloud)

Scope
Steps to restore files or entire shared folders from Hyper Backup vaults (local USB or cloud targets) on Synology DSM.

Preconditions
- You have configured Hyper Backup tasks and vaults.
- You have the Hyper Backup app and, if cloud: the encryption passphrase (if vault encryption was used). Keep the passphrase secure in your secrets manager.

Restore (GUI)
1) Open Hyper Backup in DSM
2) Click the backup task for the target you need to restore from (or use “Restore” → “Select backup task”)
3) Choose the restore point (date/time) to recover from
4) Use "File-level restore" to browse files/folders and select what you need:
   - For single files/folders: browse to the path and click "Restore" → choose restore location (original path or a temporary directory)
   - For full shared folder or system restore: use the task restore flow (may require admin rights)
5) If backup is encrypted:
   - Hyper Backup will prompt for the encryption passphrase before the restore proceeds. Enter the passphrase from your secret store.
6) Monitor the restore progress in Hyper Backup
7) After restore, verify file integrity, ownership, and permissions

Restore from cloud (Backblaze B2 or S3‑compatible)
- Cloud restores may be slower and may have different UI options
- If restore points are large, consider restoring to a local USB target or temporary volume and then rsync to the production shared folder

Restore via Hyper Backup Explorer (local mount)
- If you have the Hyper Backup vault file on a USB drive, you can mount/explore it locally using Hyper Backup Explorer (client) to retrieve files

Post-restore validation
- Open several restored files to verify contents
- Check and set correct DSM share permissions and ACLs
- If restoring a Capture One catalog/session, verify Capture One can open the restored catalog and that the CaptureOne subfolder is intact

Emergency full system recovery
- If the NAS needs a full re-image:
  1. Reinstall DSM per Synology guidance
  2. Recreate users & groups
  3. Attach backup storage and run Hyper Backup restore for shared folders & settings
  4. Reapply firewall and QuickConnect configurations (use runbooks to ensure consistency)

Notes & cautions
- Keep Hyper Backup encryption passphrases secure — losing them makes vaults unrecoverable.
- Test restores periodically to ensure backup integrity and familiarity with the procedure.
