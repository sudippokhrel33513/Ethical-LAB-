[Ethical-LAB-README (3).md](https://github.com/user-attachments/files/28243429/Ethical-LAB-README.3.md)
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
