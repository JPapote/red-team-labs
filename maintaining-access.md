# Maintaining Access

Maintaining access refers to the techniques attackers use to keep persistent control over a compromised system after the initial exploitation.

Instead of losing access when the system restarts or credentials change, attackers attempt to create mechanisms that allow them to return later without repeating the entire attack process.

Persistence techniques are commonly used in real-world attacks, red team operations, and advanced threat campaigns.

---

# Persistence Techniques

Persistence methods vary depending on the operating system and the level of privileges obtained.

Common persistence techniques include:

- adding new user accounts
- modifying startup scripts
- scheduled tasks
- SSH key persistence
- web shells
- registry modifications

---

# Creating New Users

Attackers may create a new user account with administrative privileges to maintain access.

### Linux

```bash
sudo useradd attacker
sudo passwd attacker
sudo usermod -aG sudo attacker
```
This allows the attacker to log in later with elevated privileges.

Windows
```
net user attacker Password123 /add
net localgroup administrators attacker /add
```
This creates a new administrative account on the system.

SSH Key Persistence

Attackers may add their SSH public key to a user's authorized keys file.

Example:
```
echo "attacker_public_key" >> ~/.ssh/authorized_keys
```
This allows the attacker to log in via SSH without needing the password.

Scheduled Tasks

Attackers can configure scheduled tasks that execute malicious scripts.

Linux Cron Jobs

Example:
```
crontab -e
Add a malicious task:
* * * * * /tmp/backdoor.sh
```
This runs the script every minute.

Windows Scheduled Tasks
```
schtasks /create /sc minute /mo 5 /tn "Updater" /tr C:\temp\payload.exe
```
This executes a payload every five minutes.

Web Shells

If the compromised system hosts a web server, attackers may upload a web shell to execute commands remotely.

Example file:
```
shell.php
```
This allows attackers to interact with the server through a web browser.

Web shells are commonly used in compromised web applications.

Registry Persistence (Windows)

Attackers may modify the Windows Registry to execute malicious programs at system startup.

Example:
```
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor /t REG_SZ /d "C:\temp\payload.exe"
```
This ensures the payload runs every time the user logs in.

Backdoors

Attackers sometimes install backdoors that allow them to reconnect later.

Examples include:

reverse shells
hidden services
custom malware

Backdoors provide alternative access even if the original vulnerability is patched.

Blue Team Perspective

Defenders should monitor systems for unusual persistence mechanisms such as:

unknown user accounts
modified startup scripts
suspicious scheduled tasks
unexpected SSH keys
unusual registry entries

Security monitoring tools and endpoint detection systems can help detect persistence attempts.

Regular system audits are also important to identify unauthorized changes.

Conclusion

Maintaining access is a critical phase of many cyber attacks.

Once attackers gain access, they often attempt to establish persistence to ensure they can return later without repeating the initial exploitation process.

Strong monitoring, system hardening, and regular security audits help reduce the effectiveness of persistence techniques.






























