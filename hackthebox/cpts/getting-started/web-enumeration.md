# Web Enumeration — Summary

When you see a web server (usually on **80/443**), you must **enumerate it hard** — web apps are often the main attack surface.

---

#### 1. Directory & File Brute-Forcing (Gobuster)

Use **Gobuster** (or ffuf) to find hidden directories/files:

```bash
gobuster dir -u http://10.10.10.121/ \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

Example findings:

```text
/.hta          (Status: 403)
/.htpasswd     (Status: 403)
/.htaccess     (Status: 403)
/index.php     (Status: 200)
/server-status (Status: 403)
/wordpress     (Status: 301)
```

* **200** → page exists and is accessible.
* **301/302** → redirect, still interesting.
* **403** → forbidden, but confirms the resource exists.
* `/wordpress` → WordPress, huge attack surface (plugins, themes, setup pages, etc.).

---

#### 2. DNS / Subdomain Enumeration (Gobuster DNS)

Subdomains often hide admin panels or extra apps:

```bash
gobuster dns -d inlanefreight.com \
  -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```

Example output:

```text
Found: blog.inlanefreight.com
Found: customer.inlanefreight.com
Found: my.inlanefreight.com
Found: ns1.inlanefreight.com
...
```

→ Each subdomain is another target to probe.

---

#### 3. Banner & Header Enumeration (curl)

Use **curl** to grab HTTP(S) headers and frameworks:

```bash
curl -IL https://www.inlanefreight.com
```

Example:

```text
Server: Apache/2.4.29 (Ubuntu)
Link: <https://www.inlanefreight.com/index.php/wp-json/>; rel="https://api.w.org/"
Content-Type: text/html; charset=UTF-8
```

→ Tells you web server (Apache), OS (Ubuntu), and that it’s **WordPress**.

---

#### 4. WhatWeb — Quick Tech Fingerprinting

**whatweb** fingerprints servers/frameworks fast:

```bash
whatweb 10.10.10.121
```

Example:

```text
http://10.10.10.121 [200 OK] Apache[2.4.41], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], Title[PHP 7.4.3 - phpinfo()]
```

Network-wide:

```bash
whatweb --no-errors 10.10.10.0/24
```

→ Quickly shows which IPs run Apache, nginx, PHP, WordPress, etc.

---

#### 5. Certificates (HTTPS)

Visiting `https://10.10.10.121/` and viewing the **TLS certificate** may reveal:

* **Organization name**
* **Email address**
* **Domain details**

→ Useful for social engineering / phishing (if in scope) and recon.

---

#### 6. robots.txt

Always check:

```bash
http://10.10.10.121/robots.txt
```

Example:

```text
User-agent: *
Disallow: /private
Disallow: /uploaded_files
```

Then try:

```text
http://10.10.10.121/private
```

→ Might reveal an **admin login** or other sensitive area.

---

#### 7. Page Source

View source (`Ctrl+U` in browser) and look for:

* Developer comments
* Hardcoded **test credentials**
* Hidden paths, API endpoints

Example: comment with test username/password you can use to log in.

---

**Core idea:**

For any web service you find:

1. **Bruteforce dirs/files** (Gobuster/ffuf).
2. **Enumerate subdomains** (Gobuster DNS).
3. **Grab headers & tech** (curl, whatweb).
4. **Check TLS cert, robots.txt, and source code** for leaks.

These steps alone often give you a path to auth bypass, RCE, or juicy creds.
