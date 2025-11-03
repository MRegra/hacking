# Service Scanning — Summary

**Goal:**
Find what services are running on a host (which ports, which versions), then look for misconfigurations or vulnerabilities to abuse.

* Machines are identified by **IP address**.
* Services are exposed on **ports 1–65535**.

  * **1–1023** → well-known/privileged ports.
  * **Port 0** → reserved “wildcard”, not used directly; binding to 0 means “pick an ephemeral port”.

---

### Nmap — Port & Service Scanning

**Concept:** Automate scanning of many ports to discover open services + versions.

**Default scan (top 1000 TCP ports):**

```bash
nmap 10.129.42.253
```

Example result (shortened):

```text
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

**Full, detailed scan (all ports + scripts + version):**

```bash
nmap -sC -sV -p- 10.129.42.253
```

Key takeaways from that scan:

* Services & versions (e.g. `vsftpd 3.0.3`, `OpenSSH 8.2p1`, `Apache httpd 2.4.41`, `Samba 4.6.2`).
* Extra info from scripts, e.g.:

  * `ftp-anon` → anonymous FTP login allowed.
  * `http-title` → `PHP 7.4.3 - phpinfo()` (tells you there’s a phpinfo page).

**Run a specific Nmap script:**

```bash
nmap --script smb-os-discovery.nse -p445 10.10.10.40
```

Quickly gives OS info, hostname, workgroup, etc.

---

### Banner Grabbing — Nmap & Netcat

**Nmap banner script:**

```bash
nmap -sV --script=banner -p21 10.129.42.253
```

**Manual with Netcat:**

```bash
nc -nv 10.129.42.253 21
```

Output:

```text
(UNKNOWN) [10.129.42.253] 21 (ftp) open
220 (vsFTPd 3.0.3)
```

→ Confirms FTP service and version.

---

### FTP Enumeration (port 21)

**Nmap against FTP:**

```bash
nmap -sC -sV -p21 10.129.42.253
```

Important findings:

* Service: `vsftpd 3.0.3`
* Anonymous login allowed
* `pub` directory exists

**Connecting with ftp:**

```bash
ftp -p 10.129.42.253

Name (10.129.42.253:user): anonymous
230 Login successful.

ftp> ls
drwxr-xr-x    2 ftp ftp 4096 Feb 25 19:25 pub

ftp> cd pub
ftp> ls
-rw-r--r--    1 ftp ftp 18 Feb 25 19:25 login.txt

ftp> get login.txt
ftp> exit
```

**Read downloaded file:**

```bash
cat login.txt
# admin:ftp@dmin123
```

→ Credentials you can reuse elsewhere.

---

### SMB Enumeration (port 445)

**Nmap OS/service discovery on SMB:**

```bash
nmap -A -p445 10.129.42.253
```

Gives:

* Service: `Samba smbd 4.6.2`
* Hostname: `GS-SVCSCAN`
* OS guesses: Linux variants.

**List shares with smbclient:**

```bash
smbclient -N -L \\\\10.129.42.253
```

Output:

```text
Sharename       Type
---------       ----
print$          Disk
users           Disk
IPC$            IPC
```

**Try guest access (fails):**

```bash
smbclient \\\\10.129.42.253\\users
# NT_STATUS_ACCESS_DENIED listing \*
```

**Try valid user bob:**

```bash
smbclient -U bob \\\\10.129.42.253\\users
# password: Welcome1

smb: \> ls
  bob   D   0  Thu Feb 25 16:42:23 2021

smb: \> cd bob
smb: \bob\> ls
  passwords.txt   N   156  Thu Feb 25 16:42:23 2021

smb: \bob\> get passwords.txt
smb: \bob\> exit
```

→ You’ve pulled `passwords.txt` from an SMB share.

---

### SNMP Enumeration (port 161)

**Basic snmpwalk with default community `public`:**

```bash
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0
```

Output:

```text
iso.3.6.1.2.1.1.5.0 = STRING: "gs-svcscan"
```

→ Hostname via SNMP.

**Brute-force community strings with onesixtyone:**

```bash
onesixtyone -c dict.txt 10.129.42.254
```

Example:

```text
10.129.42.254 [public] Linux gs-svcscan 5.4.0-66-generic ...
```

→ Confirms community string `public` and leaks OS details.

---

### Core Idea to Remember

Service scanning = **Nmap first**, then **targeted tools**:

* Nmap → open ports, services, versions, basic scripts.
* Netcat → quick banner checks.
* ftp → grab files on FTP.
* smbclient → list & download from SMB shares.
* snmpwalk / onesixtyone → leak hostnames, processes, configs via SNMP.

This is the starting point for almost every HTB box and real-world pentest.
