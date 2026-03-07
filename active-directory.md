# Active Directory Attacks

Active Directory (AD) is a directory service developed by Microsoft used to manage users, computers, and resources within a Windows domain.

Because it centralizes authentication and authorization, compromising Active Directory often allows attackers to control the entire network.

For this reason, Active Directory environments are one of the main targets during internal penetration tests and red team operations.

---

# Key Components of Active Directory

Understanding the main components is essential before performing attacks.

### Domain Controller (DC)

A Domain Controller is a server that manages authentication and security policies within the domain.

Responsibilities include:

- authenticating users
- enforcing policies
- storing directory information

If an attacker compromises a Domain Controller, they can control the entire domain.

---

### Domain

A domain is a group of computers and users managed by Active Directory.

Example:
company.local

Users authenticate against the domain to access network resources.

---

### Users and Groups

Users represent individual accounts.

Groups are used to assign permissions efficiently.

Example privileged groups:

- Domain Admins
- Enterprise Admins
- Administrators

Members of these groups have high privileges in the network.

---

# Active Directory Enumeration

Once inside a network, attackers begin gathering information about the domain.

### Basic Domain Information

```powershell
whoami
whoami /groups
whoami /priv
```
Domain Information
```
systeminfo
nltest /domain_trusts
net config workstation
```
Domain Users
```
net user /domain
```
Domain Groups
```
net group /domain
```
Domain Admins
```
net group "Domain Admins" /domain
```
SMB is commonly used in Windows environments and can reveal useful information.

Example:
```
smbclient -L //<target-ip> -N
```
This may reveal:

shared folders
accessible resources
internal file servers

Kerberoasting

Kerberoasting is a technique used to extract service account password hashes from Active Directory.

Attackers request service tickets for accounts running services.

These tickets can then be cracked offline to recover the password.

Steps

Request Kerberos service tickets
Extract ticket hashes
Crack them offline

Tools commonly used:

Impacket
Rubeus
Hashcat

Pass-the-Hash

Pass-the-Hash allows attackers to authenticate using a password hash instead of the plaintext password.

If an attacker obtains NTLM hashes, they may authenticate to other systems without knowing the real password.

Common tools:

Mimikatz
Impacket
CrackMapExec

BloodHound

BloodHound is a tool used to analyze relationships in Active Directory.

It collects data about:

users
groups
permissions
sessions

This allows attackers to visualize attack paths that lead to domain compromise.

Common workflow:

Collect data using SharpHound
Import data into BloodHound
Analyze privilege escalation paths

Lateral Movement in Active Directory

After gaining access to one machine, attackers attempt to move across the network.

Common methods include:

SMB authentication
Remote PowerShell
Windows Management Instrumentation (WMI)
Remote Desktop Protocol (RDP)

The goal is to reach high-value systems such as Domain Controllers.

Blue Team Perspective

Defenders should monitor for suspicious activity such as:

unusual Kerberos ticket requests
authentication attempts across multiple systems
abnormal SMB access
credential dumping attempts

Important logs:

Windows Security Event Logs
Kerberos authentication logs
Domain Controller logs

Security tools such as SIEM and EDR help detect Active Directory attacks.

Conclusion

Active Directory is one of the most critical components of enterprise networks.

Because it manages authentication and access control, compromising Active Directory often leads to full network compromise.

Proper monitoring, privilege management, and network segmentation are essential to defend against Active Directory attacks.




















































