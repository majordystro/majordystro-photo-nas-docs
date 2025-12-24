# Runbook / Checklist: Remote‑access hardening (QuickConnect + DSM)

Use this checklist to harden remote access and reduce exposure.

Preparation
- Identify accounts that require remote access and limit them.
- Keep a secure copy of all admin credentials and 2FA recovery codes in an approved password manager.

Checklist
- QuickConnect
  - [ ] Limit QuickConnect to only required services (Control Panel → QuickConnect)
  - [ ] Ensure QuickConnect account is protected by a strong, unique password and 2FA
  - [ ] Audit QuickConnect access logs periodically
- DSM accounts
  - [ ] Disable default `admin` account or rename and remove its use
  - [ ] Enforce strong password policy for all accounts
  - [ ] Enable 2FA for all remote-capable accounts
  - [ ] Configure Account Protection / Auto-block for brute force protection
- Firewall & network
  - [ ] Enable DSM Firewall; restrict incoming traffic to trusted subnets and QuickConnect ranges as needed
  - [ ] Disable unused services (FTP, WebDAV, etc.) or restrict them by IP
  - [ ] If remote admin needed for sensitive tasks, require VPN (WireGuard/OpenVPN) to access DSM
- SSH
  - [ ] Disable SSH if not required
  - [ ] If SSH required: enforce key-based auth, disable password auth, and restrict by firewall rules
  - [ ] Keep an audit of SSH keys allowed to connect
- SMB
  - [ ] Disable SMB1, set minimum SMB protocol to SMB2 or higher
  - [ ] Disable NTLMv1
  - [ ] Consider enabling SMB signing
  - [ ] Ensure share permissions and ACLs follow least privilege
- Packages & updates
  - [ ] Remove or disable unused packages
  - [ ] Keep DSM and installed packages up-to-date; schedule maintenance windows
- Logging & alerts
  - [ ] Enable Notifications for critical events (login failures, auto-block triggers, disk health)
  - [ ] Configure Log Center retention and archive logs off NAS when appropriate
- Testing & validation
  - [ ] Test 2FA recovery scenarios for locked accounts
  - [ ] Periodically attempt a remote connection from an external network (controlled test) to validate settings
  - [ ] Run a restore test from Hyper Backup to validate recovery posture

Response actions
- If suspicious activity detected:
  - [ ] Immediately disable remote access (QuickConnect, open firewall rules)
  - [ ] Change admin passwords and revoke API/third-party keys
  - [ ] Preserve logs and snapshot metadata for investigation
  - [ ] Restore from known-good backup if needed

Notes
- QuickConnect is convenient but exposes fewer opportunities for granular network-level access control than a site VPN. Plan a migration to VPN-based administration for sensitive operations.
