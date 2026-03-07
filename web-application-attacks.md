# Web Application Attacks

Web applications are one of the most common attack surfaces in modern systems.  
Many organizations rely on web services for authentication, data storage, and user interaction.

Because of this, vulnerabilities in web applications can lead to serious security breaches, including data leaks, remote code execution, and account compromise.

Understanding common web vulnerabilities is essential for both penetration testers and security professionals.

---

# Common Web Vulnerabilities

Some of the most common vulnerabilities found in web applications include:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- File Upload Vulnerabilities
- Authentication Bypass
- Insecure Direct Object References (IDOR)

Many of these vulnerabilities are documented in the OWASP Top 10.

---

# SQL Injection

SQL Injection occurs when user input is not properly sanitized before being included in a database query.

Attackers can manipulate queries to access or modify database information.

Example vulnerable query:

```sql
SELECT * FROM users WHERE username = '$user' AND password = '$pass';
```
Example attack input:
```
' OR '1'='1
```
This may allow attackers to bypass authentication and gain unauthorized access.

Tools commonly used:

sqlmap
Burp Suite
manual payload testing

Cross-Site Scripting (XSS)

Cross-Site Scripting allows attackers to inject malicious JavaScript into web pages viewed by other users.

Types of XSS:

Stored XSS
Reflected XSS
DOM-based XSS

Example payload:
```
<script>alert('XSS')</script>
```
Potential impacts:

session hijacking
credential theft
malicious redirects

Command Injection

Command Injection occurs when user input is executed as part of a system command.

Example vulnerable code:
```
ping <user_input>
```
Example payload:
```
127.0.0.1; id
```
This could allow an attacker to execute arbitrary system commands.

File Upload Vulnerabilities

Many web applications allow users to upload files such as images or documents.

If file validation is not properly implemented, attackers may upload malicious files such as web shells.

Example:
```
shell.php
```
Once uploaded, the attacker may execute commands on the server.

Common defenses include:

file type validation
restricting executable permissions
storing uploads outside the web root

Example:
```
https://example.com/account?id=1001
```
An attacker may modify the parameter:
```
https://example.com/account?id=1002
```
If authorization checks are missing, the attacker may access another user's data.

Authentication Bypass

Weak authentication mechanisms may allow attackers to bypass login systems.

Common causes include:

weak session management
predictable tokens

logic flaws in authentication workflows

Attackers often test login forms for:

SQL injection
default credentials
brute force attacks

Tools for Web Application Testing

Common tools used during web penetration testing include:

Burp Suite
OWASP ZAP
sqlmap
dirsearch
ffuf

These tools help identify vulnerabilities such as hidden endpoints, injection points, and misconfigurations.

Blue Team Perspective

Defending web applications requires several security measures:
input validation
parameterized queries
secure authentication mechanisms
proper access control
regular security testing
Using Web Application Firewalls (WAF) and security monitoring can also help detect malicious activity.

Conclusion

Web applications are a primary target for attackers due to their exposure to the internet.

Understanding common vulnerabilities and how they are exploited is essential for identifying and mitigating security risks.

Secure coding practices, regular security testing, and proper monitoring significantly reduce the risk of successful attacks.




























