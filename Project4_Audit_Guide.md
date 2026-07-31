# Project 4 — System Vulnerability Checklist (Audit Guide)

Run the checks below **on your own PC**. Note down what you find — you'll use it to fill the report template at the bottom.

---

## Step 1: Identity Front Door (Passwords & MFA)

| Check | Windows | macOS / Linux |
|---|---|---|
| Password set? | `net user %username%` | `passwd -S $(whoami)` |
| MFA enabled (email, OS login) | Check Settings > Accounts > Sign-in options | Check System Settings > Touch ID & Password |

**Look for:** weak/reused passwords, no MFA/2FA on your OS login, email, or important accounts.

---

## Step 2: Software Decay & Patch Management

| Check | Windows | macOS | Linux |
|---|---|---|---|
| OS updates status | `Get-WindowsUpdateLog` or Settings > Windows Update | `softwareupdate -l` | `sudo apt update && apt list --upgradable` |
| Browser version | Browser > About | same | same |
| Antivirus status | `Get-MpComputerStatus` (PowerShell) | XProtect (built-in) | `clamscan --version` if installed |

**Look for:** OS not updated in weeks/months, outdated browser, no active antivirus/Defender.

---

## Step 3: Human Perimeter (Accounts & Physical Hygiene)

| Check | Windows | macOS / Linux |
|---|---|---|
| Guest account status | `net user guest` | `dscl . -list /Users` (macOS) / `cat /etc/passwd` (Linux) |
| Admin group members | `net localgroup administrators` | `dscl . -read /Groups/admin GroupMembership` |
| Screen lock timeout | Settings > Lock screen > timeout | System Settings > Lock Screen |

**Look for:** Guest account enabled, extra/unnecessary admin accounts, no auto-lock, passwords saved in plain text files/sticky notes.

---

## Step 4: Network & Endpoint Hygiene

| Check | Windows | macOS | Linux |
|---|---|---|---|
| Firewall status | `netsh advfirewall show allprofiles` | `sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate` | `sudo ufw status` |
| Disk encryption | `manage-bde -status` (BitLocker) | `fdesetup status` (FileVault) | `lsblk -f` (check for LUKS) |

**Look for:** Firewall off, disk not encrypted.

---

## CVSS-Style Risk Ranking (for scoring your findings)

| Risk | Example |
|---|---|
| Critical (9.0–10.0) | Remote code execution, no password on an account |
| High | No MFA, unpatched browser |
| Medium | Missing firewall rule, over-permissive file permissions |
| Low | Outdated non-essential app |

---

## Report Template (fill this in and submit)

```
VULNERABILITY REPORT — [Your Name]
Date: [Date] | Machine: [OS + version]

SECTION 1: FLAWS FOUND
1. [Flaw] — CVSS: [score/level] — [1-line description]
2. [Flaw] — CVSS: [score/level] — [1-line description]
3. [Flaw] — CVSS: [score/level] — [1-line description]

SECTION 2: REMEDIATION ACTIONS
1. [What you did to fix flaw 1, with command used]
2. [What you did to fix flaw 2, with command used]
3. [What you did to fix flaw 3, with command used]

SECTION 3: HARDENED VERIFICATION
- [Terminal output/screenshot proving firewall is ON]
- [Terminal output/screenshot proving disk encryption is ON]
- [Any other proof of fixed state]
```
