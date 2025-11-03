Here’s a compact, no-fluff summary for your notes:

---

### Common Terms — Summary

**Shell:**

* A **shell** is a command-line interface used to interact with an OS (e.g., Bash, Zsh, PowerShell).
* “Getting a shell” means gaining command execution on a target system.
* **Types of shells:**

  * **Reverse shell:** target connects back to your listener.
  * **Bind shell:** attacker connects to a port opened on the target.
  * **Web shell:** commands executed via a web interface (e.g., uploaded PHP script).

**Port:**

* A **port** is a virtual communication endpoint for networked services (like doors into a system).
* Two main types:

  * **TCP** – connection-oriented (reliable).
  * **UDP** – connectionless (faster, less reliable).
* **Common ports:**

  * 21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 80 HTTP, 443 HTTPS, 445 SMB, 3389 RDP.
* Knowing port numbers and associated services speeds up enumeration and attack prioritization.

**Web Server:**

* A **web server** handles HTTP(S) traffic between browsers and backend applications, usually on ports 80 or 443.
* Web apps are a major attack surface — they’re public-facing and often vulnerable.

**OWASP Top 10 (Web Vulnerabilities):**

1. **Broken Access Control** – improper restrictions let users access unauthorized data/actions.
2. **Cryptographic Failures** – weak or missing encryption exposing sensitive data.
3. **Injection** – unsanitized input leading to command/SQL/LDAP injections.
4. **Insecure Design** – flaws due to poor security architecture.
5. **Security Misconfiguration** – insecure defaults, verbose errors, open storage, etc.
6. **Vulnerable/Outdated Components** – using old or unsupported libraries/software.
7. **Auth Failures** – issues with login, session handling, or identity checks.
8. **Integrity Failures** – using untrusted or tampered dependencies.
9. **Logging/Monitoring Failures** – no detection or response to breaches.
10. **SSRF** – server requests to unintended internal/external locations.

**Key takeaway:**
Understand how shells, ports, and web servers function — these are the foundation of most attacks and enumerations. Mastering the OWASP Top 10 prepares you to identify and exploit the most common web vulnerabilities.

---
