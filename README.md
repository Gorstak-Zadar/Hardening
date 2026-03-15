# 🛡️ Windows Security Hardening Script

A comprehensive PowerShell script designed to harden Windows systems against common attack vectors, including those commonly seen in penetration testing platforms like HackTheBox. This script implements defense-in-depth security measures to protect against unauthorized access, credential theft, privilege escalation, and lateral movement attacks.

## ⚠️ Disclaimer

**This script makes significant system-wide security changes.** Always test in a non-production environment first. Some changes may affect system functionality or require a reboot. Use at your own risk.

## ✨ Features

### 🎯 Comprehensive Protection

- **Network Attack Prevention**: Blocks SMB enumeration, RDP/WinRM brute-force attacks, and common attack ports
- **Credential Protection**: Prevents credential dumping (Mimikatz-style attacks), pass-the-hash, and stores passwords securely
- **Privilege Escalation Prevention**: Detects and prevents unquoted service paths, COM hijacking, and AlwaysInstallElevated exploitation
- **Lateral Movement Mitigation**: Hardens WMI, disables admin shares, prevents LLMNR/NBT-NS poisoning
- **Drive-by Download Protection**: Enforces SmartScreen, DEP, browser hardening, and network protection
- **Physical Security**: Protects against Sticky Keys exploits, USB malware, and unauthorized physical access
- **Enhanced Auditing**: Comprehensive security logging for forensics and detection

### 🔒 Security Measures Implemented

- ✅ LSA Protection (prevents credential dumping)
- ✅ SMB signing enforcement
- ✅ NTLMv2-only authentication
- ✅ Anonymous SMB enumeration disabled
- ✅ Windows Defender ASR rules enabled
- ✅ Enhanced audit policies
- ✅ Account lockout policies
- ✅ Screen saver lock (15-minute timeout)
- ✅ Browser security hardening
- ✅ COM hijacking prevention
- ✅ BCD bootkit detection
- ✅ And [much more...](#complete-protection-coverage)

## 📋 Requirements

- **Windows**: Windows 10, Windows 11, or Windows Server 2016+
- **PowerShell**: Version 5.1 or higher (included with Windows)
- **Privileges**: Must run as **Administrator**
- **Modules**: Some features require Windows Defender PowerShell module (optional, script handles gracefully if unavailable)

## 🚀 Quick Start

1. **Download the script:**
   ```powershell
   # Clone the repository or download windows-security-hardening.ps1
   ```

2. **Run PowerShell as Administrator:**
   - Right-click on PowerShell
   - Select "Run as Administrator"

3. **Execute the script:**
   ```powershell
   .\windows-security-hardening.ps1
   ```

4. **Review the output** and reboot if prompted for some changes to take full effect.

## 📖 Detailed Usage

### Basic Execution

```powershell
# Run with default settings (applies all hardening measures)
.\windows-security-hardening.ps1
```

### Verification Mode

```powershell
# Check current security settings without making changes
.\windows-security-hardening.ps1 -VerifyOnly
```

### Execution Policy

If you encounter execution policy errors:

```powershell
# Temporarily allow script execution for current session
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

# Then run the script
.\windows-security-hardening.ps1
```

## 🎯 Complete Protection Coverage

### Network Attack Prevention

| Attack Vector | Protection |
|--------------|------------|
| SMB Enumeration | Anonymous access disabled, SMBv1 disabled, admin shares disabled |
| RDP Brute-Force | NLA enforced, port blocking, firewall rules |
| WinRM Attacks | HTTPS only, authentication enforced, port blocking |
| EternalBlue (MS17-010) | SMBv1 disabled, firewall rules, patches recommended |
| Port Scanning | Common attack ports blocked (3389, 5985, 445, 139, etc.) |

### Credential Theft Prevention

| Attack Vector | Protection |
|--------------|------------|
| Mimikatz/LSASS Dumping | LSA Protection enabled, WDigest disabled |
| Pass-the-Hash | LM hash storage disabled, NTLMv2 enforced |
| Credential Caching | Cached logon credentials disabled |
| Memory Dumps | WDigest disabled, enhanced memory protection |
| SAM Database Access | Registry ACL restrictions |

### Privilege Escalation Prevention

| Attack Vector | Protection |
|--------------|------------|
| Unquoted Service Paths | Detection and reporting |
| COM Hijacking | Registry ACL hardening |
| AlwaysInstallElevated | Registry keys disabled |
| DLL Hijacking | System path permissions reviewed |
| Token Impersonation | UAC and privilege restrictions |

### Lateral Movement Mitigation

| Attack Vector | Protection |
|--------------|------------|
| WMI Attacks | WMI hardening, remote access restrictions |
| Admin Shares (C$, D$) | Auto-sharing disabled |
| LLMNR/NBT-NS Poisoning | Protocols disabled |
| SMB Relay Attacks | SMB signing enforced |
| PowerShell Remoting | Restricted and hardened |

### Drive-by Download Protection

| Attack Vector | Protection |
|--------------|------------|
| Malicious Websites | SmartScreen enforced, network protection enabled |
| Browser Exploits | ActiveX disabled, scripting restricted, DEP enabled |
| Browser Extensions | Installation blocked (whitelist-only) |
| Java/Flash Exploits | Disabled in browser |
| WebRTC IP Leaks | Firefox WebRTC disabled |

### Physical Security

| Attack Vector | Protection |
|--------------|------------|
| Sticky Keys Exploit | sethc.exe protected from replacement |
| USB Malware | USB write protection configured |
| BadUSB Attacks | USB access restrictions |
| Accessibility Tool Abuse | All accessibility executables protected |

### Advanced Protections

- **Windows Defender ASR Rules**: 17 attack surface reduction rules enabled
- **Enhanced Audit Policies**: Comprehensive logging for security events
- **BCD Bootkit Detection**: Scans for suspicious boot entries
- **Screen Saver Lock**: Automatic lock after 15 minutes of inactivity
- **Account Lockout**: Locks after 5 failed attempts for 30 minutes
- **Password Policy**: 12-character minimum, complexity enabled

## 📊 What the Script Does

### Services Hardened

- Disables unnecessary services (RemoteRegistry, RemoteAccess, Spooler, etc.)
- Stops and disables Xbox services if gaming is not needed
- Configures service startup types for security

### Registry Changes

- **UAC**: Enabled and enforced
- **Credential Protection**: LSA Protection, WDigest disabled
- **Network Security**: NTLMv2 enforced, anonymous access disabled
- **Browser Security**: ActiveX, scripting, and Java restrictions
- **Error Reporting**: Disabled to prevent data leakage

### Firewall Configuration

- Enables Windows Firewall on all profiles
- Blocks common attack ports (RDP, WinRM, SMB, etc.)
- Enables firewall logging
- Blocks inbound connections by default

### Windows Defender Settings

- Real-time protection enabled
- Controlled folder access enabled (ransomware protection)
- Network protection enabled
- PUA (Potentially Unwanted Application) protection enabled
- ASR (Attack Surface Reduction) rules enabled

### Audit Policies

- Account Management events
- Logon/Logoff events
- Credential Validation
- File System and Registry access
- Policy Changes
- System Integrity events

## 🔄 When to Re-Run

The script settings **persist permanently**, but you should re-run in these cases:

- ✅ **After major Windows Updates** (settings may be reset)
- ✅ **After System Restore or Windows Refresh/Reset**
- ✅ **If you suspect your system may have been compromised**
- ✅ **Periodically for compliance/audit purposes** (monthly recommended)
- ✅ **After installing software that modifies system settings**

## ⚙️ Script Behavior

- **Fully Automated**: No user interaction required
- **Idempotent**: Safe to run multiple times
- **Error Handling**: Continues even if some steps fail
- **Modular Design**: Each protection is independent
- **Comprehensive Logging**: Clear output showing what's being hardened

## 🛠️ Troubleshooting

### "Execution Policy" Error

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### "Access Denied" Errors

- Ensure you're running PowerShell **as Administrator**
- Check if Group Policy is overriding local settings
- Verify you have sufficient privileges

### Windows Defender Commands Not Found

- This is normal on Windows Server editions or systems without Defender
- The script will skip Defender steps gracefully
- Other security measures still apply

### Some Settings Not Applied

- **Reboot required**: Some registry changes need a reboot
- **Group Policy**: Domain policies may override local settings
- **Service dependencies**: Some services may restart themselves

### BCD Warnings (False Positives)

The script flags `.efi` boot files as "suspicious" because it looks for `winload.exe`. On UEFI systems, `winload.efi` is normal. These warnings can be safely ignored on UEFI systems.

## 📝 Important Notes

### Before Running

- ⚠️ **Backup your system** or create a restore point
- ⚠️ **Test in a non-production environment** first
- ⚠️ **Review the script** to understand what changes will be made
- ⚠️ **Document your baseline** for rollback if needed

### After Running

- 🔄 **Reboot your system** for all changes to take effect
- ✅ **Verify firewall rules** are working correctly
- ✅ **Test network connectivity** if you use file sharing
- ✅ **Review Windows Event Logs** for any issues

### System Compatibility

- ✅ **Home PCs**: Fully supported
- ✅ **Workstations**: Fully supported
- ⚠️ **Domain-joined systems**: Some settings may conflict with Group Policy
- ⚠️ **Server editions**: Windows Defender features may not be available

## 🔐 Security Best Practices

This script implements many security measures, but you should also:

1. **Keep Windows Updated**: Install security patches regularly
2. **Use Standard User Accounts**: Don't run as Administrator daily
3. **Enable Multi-Factor Authentication (MFA)**: Wherever possible
4. **Use a Password Manager**: Strong, unique passwords for all accounts
5. **Be Cautious with Emails**: Don't open suspicious attachments
6. **Enable BitLocker**: Full disk encryption for offline attack prevention
7. **Enable Secure Boot**: In UEFI/BIOS settings
8. **Set BIOS/UEFI Password**: Prevents boot from USB attacks
9. **Monitor Event Logs**: Regularly review security events
10. **Backup Regularly**: Important files should be backed up

## 🧪 Testing

Tested on:
- ✅ Windows 10 (all editions)
- ✅ Windows 11 (all editions)
- ✅ Windows Server 2016/2019/2022
- ✅ Domain-joined and standalone systems

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Please feel free to:

1. Open an issue for bugs or feature requests
2. Submit a pull request with improvements
3. Share your testing results and feedback

## 📄 License

This script is provided as-is for educational and security hardening purposes. Use at your own risk.

## 🙏 Acknowledgments

- Inspired by common attack vectors seen in penetration testing platforms
- Based on Microsoft security recommendations and best practices
- Incorporates protections from various security hardening guides

## 📧 Support

For issues, questions, or security concerns:
- Open an issue on GitHub
- Review the troubleshooting section above
- Check Windows Event Logs for detailed error information

---

**Remember**: Security is an ongoing process. This script provides a strong foundation, but regular updates, monitoring, and good security practices are essential for maintaining a secure system.

**Stay secure! 🛡️**

---

## Disclaimer

**NO WARRANTY.** THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. EXCEPT WHEN OTHERWISE STATED IN WRITING THE COPYRIGHT HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.

**Limitation of Liability.** IN NO EVENT UNLESS REQUIRED BY APPLICABLE LAW OR AGREED TO IN WRITING WILL ANY COPYRIGHT HOLDER, OR ANY OTHER PARTY WHO MODIFIES AND/OR CONVEYS THE PROGRAM AS PERMITTED ABOVE, BE LIABLE TO YOU FOR DAMAGES, INCLUDING ANY GENERAL, SPECIAL, INCIDENTAL OR CONSEQUENTIAL DAMAGES ARISING OUT OF THE USE OR INABILITY TO USE THE PROGRAM (INCLUDING BUT NOT LIMITED TO LOSS OF DATA OR DATA BEING RENDERED INACCURATE OR LOSSES SUSTAINED BY YOU OR THIRD PARTIES OR A FAILURE OF THE PROGRAM TO OPERATE WITH ANY OTHER PROGRAMS), EVEN IF SUCH HOLDER OR OTHER PARTY HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.
