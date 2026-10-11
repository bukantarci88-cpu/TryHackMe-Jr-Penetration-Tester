# 🎯 TryHackMe Jr Penetration Tester

Hands-on labs, technical notes, and challenge write-ups from the **TryHackMe Jr Penetration Tester** learning path. This repository covers reconnaissance, network scanning, web application security, vulnerability research, exploitation, privilege escalation, Active Directory testing, Python scripting, and professional penetration testing methodology.

## 🎯 About This Repository

This repository organizes practical exercises, technical notes, and challenge write-ups by learning-path topic and room. It documents key concepts, commands, testing workflows, findings, and lessons from authorized lab environments.

The goal is to build a structured cybersecurity portfolio demonstrating practical security testing knowledge, familiarity with industry-standard tools, and a methodical approach to identifying and validating vulnerabilities.

## 📚 Learning Path Topics

1. **[Start Your Cyber Security Journey](./01-Start-Your-Cyber-Security-Journey/)** — offensive and defensive security fundamentals, plus search skills.
2. **[Penetration Testing Foundations](./02-Penetration-Testing-Foundations/)** — guided engagements, penetration testing concepts, the Cyber Kill Chain, and testing frameworks.
3. **[Network Reconnaissance](./03-Network-Reconnaissance/)** — passive and active reconnaissance, protocols, and network services.
4. **[Nmap](./04-Nmap/)** — live host discovery, basic and advanced port scans, and post-scan enumeration.
5. **[Web Application Security Fundamentals](./05-Web-Application-Security-Fundamentals/)** — application mapping, content discovery, modern web stacks, and web server attacks.
6. **[Burp Suite](./06-Burp-Suite/)** — fundamentals, Repeater, Intruder, other modules, and extensions.
7. **[Web Application Vulnerabilities I](./07-Web-Application-Vulnerabilities-I/)** — SQL injection, CSRF, XSS, SSRF, IDOR, and the Recruit challenge.
8. **[Web Application Vulnerabilities II](./08-Web-Application-Vulnerabilities-II/)** — session management, broken authentication, file inclusion, command injection, API testing, and the Support challenge.
9. **[Vulnerability Knowledge](./09-Vulnerability-Knowledge/)** — vulnerability databases, scanning tools, identification techniques, and selected vulnerability case studies.
10. **[OWASP Top 10 (2025)](./10-OWASP-Top-10-2025/)** — IAAA failures, application design flaws, and insecure data handling.
11. **[Password Attacks](./11-Password-Attacks/)** — phishing, Hydra, wordlists, password cracking, and the Checkmate challenge.
12. **[Metasploit and Exploitation](./12-Metasploit-Exploitation/)** — Metasploit basics, scanning, exploitation, post-exploitation, payload generation, shells, and listeners.
13. **[Privilege Escalation](./13-Privilege-Escalation/)** — host configuration reviews, Linux and Windows enumeration, escalation techniques, automation, and practical challenges.
14. **[Active Directory Security Testing Basics](./14-Active-Directory-Security-Testing-Basics/)** — AD fundamentals, authentication, domain breaching, enumeration, credential harvesting, and lateral movement.
15. **[Specialized Domains](./15-Specialized-Domains/)** — wireless, mobile, cloud, LLM, blue-team, and DevSecOps security.
16. **[Python Scripting Basics](./16-Python-Scripting-Basics/)** — Python fundamentals, script development, and penetration testing automation.
17. **[Pentesting Methodologies and Reporting](./17-Pentesting-Methodologies-Reporting/)** — threat modeling, planning, scoping, reporting, and retesting.
18. **[Jr Pentester Challenges](./18-Jr-Pentester-Challenges/)** — practical challenges applying reconnaissance, enumeration, exploitation, and security testing workflows.

## 🧰 Tools & Technologies

| Category | Tools / Concepts |
|---|---|
| Reconnaissance | Passive and active reconnaissance, information gathering |
| Network Scanning | Nmap, host discovery, port scanning, service enumeration |
| Web Testing | Burp Suite, HTTP request/response analysis |
| Web Vulnerabilities | SQL injection, CSRF, XSS, SSRF, IDOR, file inclusion, command injection |
| Vulnerability Research | Vulnerability databases, scanning tools, CVE analysis |
| Password Security | Hydra, wordlists, password cracking, phishing scenarios |
| Exploitation | Metasploit Framework, payloads, shells, listeners |
| Privilege Escalation | Linux and Windows enumeration, configuration reviews |
| Directory Services | Active Directory enumeration, authentication, lateral movement |
| Scripting | Python fundamentals and automation |
| Methodology | Threat modeling, scoping, reporting, retesting |
| Specialized Security | Wireless, mobile, cloud, LLM security, Blue Team, DevSecOps |

## 🔍 Skills Practiced

- Reconnaissance, host discovery, and service enumeration
- Network scanning and Nmap scan analysis
- Web application mapping and content discovery
- Testing common web vulnerabilities in authorized lab environments
- Using Burp Suite to inspect and manipulate HTTP traffic
- Researching vulnerabilities and reviewing CVE information
- Password attack concepts and password-cracking workflows
- Exploitation and post-exploitation fundamentals
- Linux and Windows privilege escalation
- Active Directory enumeration and attack-path concepts
- Python scripting for repeatable testing tasks
- Threat modeling, scoping, reporting, and retesting
- Applying structured methodology to practical challenges

## 📈 Penetration Testing Workflow

**Reconnaissance → Enumeration → Vulnerability Analysis → Validation → Exploitation → Privilege Escalation → Reporting**

The workflow emphasizes methodical assessment, validation of findings in authorized environments, evaluation of security impact, and clear remediation recommendations.

## 💼 Practical Portfolio Evidence

This repository serves as a structured record of hands-on cybersecurity practice, including:

- Technical notes and command references for security tools.
- Lab-based vulnerability identification and validation.
- Linux, Windows, and Active Directory security testing.
- Vulnerability research and selected CVE case studies.
- Practical challenge write-ups documenting testing methodology and findings.

Individual room folders contain relevant notes, commands, screenshots, or technical explanations where available.

## 📂 Repository Structure

<details>
<summary>📁 View the complete repository structure</summary>

```text
TryHackMe-Jr-Penetration-Tester/
├── 01-Start-Your-Cyber-Security-Journey/
│   ├── 1-1-Offensive-Security-Intro/
│   ├── 1-2-Defensive-Security-Intro/
│   └── 1-3-Search-Skills/
├── 02-Penetration-Testing-Foundations/
│   ├── 2-1-Guided-Pentest-Web/
│   ├── 2-2-Guided-Pentest-Infrastructure/
│   ├── 2-3-Dive-Into-Pentesting/
│   ├── 2-4-Cyber-Kill-Chain/
│   └── 2-5-Penetration-Testing-Frameworks/
├── 03-Network-Reconnaissance/
│   ├── 3-1-Passive-Reconnaissance/
│   ├── 3-2-Active-Reconnaissance/
│   ├── 3-3-Protocols-Servers/
│   └── 3-4-Protocols-Servers-2/
├── 04-Nmap/
│   ├── 4-1-Nmap-Live-Host-Discovery/
│   ├── 4-2-Nmap-Basic-Port-Scans/
│   ├── 4-3-Nmap-Advanced-Port-Scans/
│   └── 4-4-Nmap-Post-Port-Scans/
├── 05-Web-Application-Security-Fundamentals/
│   ├── 5-1-Walking-An-Application/
│   ├── 5-2-Content-Discovery/
│   ├── 5-3-Modern-Web-Stacks/
│   ├── 5-4-Web-Server-Attacks-I/
│   └── 5-5-Web-Server-Attacks-II/
├── 06-Burp-Suite/
│   ├── 6-1-Burp-Suite-The-Basics/
│   ├── 6-2-Burp-Suite-Repeater/
│   ├── 6-3-Burp-Suite-Intruder/
│   ├── 6-4-Burp-Suite-Other-Modules/
│   └── 6-5-Burp-Suite-Extensions/
├── 07-Web-Application-Vulnerabilities-I/
│   ├── 7-1-SQL-Injection-Introduction/
│   ├── 7-2-CSRF-Introduction/
│   ├── 7-3-XSS-Introduction/
│   ├── 7-4-Intro-SSRF/
│   ├── 7-5-IDOR/
│   └── 7-6-Recruit/
├── 08-Web-Application-Vulnerabilities-II/
│   ├── 8-1-Session-Management/
│   ├── 8-2-Broken-Authentication/
│   ├── 8-3-File-Inclusion/
│   ├── 8-4-Command-Injection/
│   ├── 8-5-API-Pentesting/
│   └── 8-6-Support/
├── 09-Vulnerability-Knowledge/
│   ├── 9-1-Understanding-Vulnerability-Database/
│   ├── 9-2-Vulnerability-Scanning-Tools/
│   ├── 9-3-Basic-Vulnerability-Identification-Techniques/
│   ├── 9-4-NoScope-Finding-RCE/
│   ├── 9-5-n8n-CVE-2025-68613/
│   ├── 9-6-AD-BadSuccessor/
│   ├── 9-7-CVE-2026-46300-Fragnesia/
│   └── 9-8-CVE-2026-42945-Nginx-Rift/
├── 10-OWASP-Top-10-2025/
│   ├── 10-1-OWASP-Top-10-2025-IAAA-Failures/
│   ├── 10-2-OWASP-Top-10-2025-Application-Design-Flaws/
│   └── 10-3-OWASP-Top-10-2025-Insecure-Data-Handling/
├── 11-Password-Attacks/
│   ├── 11-1-Phishing-Basics/
│   ├── 11-2-Hydra/
│   ├── 11-3-Introduction-Wordlists/
│   ├── 11-4-Password-Cracking/
│   └── 11-5-Checkmate/
├── 12-Metasploit-Exploitation/
│   ├── 12-1-Metasploit-Basics/
│   ├── 12-2-Metasploit-Scanning-Exploitation/
│   ├── 12-3-Metasploit-Post-Exploitation/
│   ├── 12-4-Metasploit-Payload-Generation/
│   ├── 12-5-Exploitation-Weaponisation/
│   ├── 12-6-Shells-Listeners-Fundamentals/
│   └── 12-7-Shell-Payload-Generation-Delivery/
├── 13-Privilege-Escalation/
│   ├── 13-1-Host-Server-Configuration-Reviews/
│   ├── 13-2-Linux-Privilege-Escalation-Enumeration/
│   ├── 13-3-Linux-Privilege-Escalation-Basics/
│   ├── 13-4-Linux-Privilege-Escalation-Automation/
│   ├── 13-5-Windows-Privilege-Escalation/
│   ├── 13-6-Jump/
│   └── 13-7-Windows-Jump/
├── 14-Active-Directory-Security-Testing-Basics/
│   ├── 14-1-Active-Directory-Basics/
│   ├── 14-2-Intro-AD-Authentication/
│   ├── 14-3-Intro-AD-Breaching/
│   ├── 14-4-AD-Basic-Enumeration/
│   ├── 14-5-AD-Authenticated-Enumeration/
│   ├── 14-6-Intro-Credential-Harvesting/
│   ├── 14-7-Intro-AD-Lateral-Movement/
│   ├── 14-8-Proxy/
│   └── 14-9-Forward/
├── 15-Specialized-Domains/
│   ├── 15-1-Wireless-Security/
│   ├── 15-2-Mobile-Application-Security/
│   ├── 15-3-Cloud-Security-Fundamentals/
│   ├── 15-4-LLM-Pentesting/
│   ├── 15-5-The-Blue-Team-Perspective/
│   └── 15-6-DevSecOps-Basics/
├── 16-Python-Scripting-Basics/
│   ├── 16-1-Python-Simple-Demo/
│   ├── 16-2-Python-Core-Concepts/
│   ├── 16-3-Python-Building-Scripts/
│   └── 16-4-Python-Pentesting-Scripts/
├── 17-Pentesting-Methodologies-Reporting/
│   ├── 17-1-Threat-Modeling-for-Pentesters/
│   ├── 17-2-Planning-Scoping/
│   ├── 17-3-Writing-Pentest-Reports/
│   └── 17-4-Re-Testing/
├── 18-Jr-Pentester-Challenges/
│   ├── 18-1-Domino/
│   ├── 18-2-Silent-Monitor/
│   ├── 18-3-Dead-Drop/
│   ├── 18-4-Operation-Promotion/
│   ├── 18-5-Operation-Coldstart/
│   ├── 18-6-Net-Sec-Challenge/
│   └── 18-7-Interceptor/
└── README.md
```

</details>

## 🎓 Learning Path

**Platform:** TryHackMe  
**Path:** Jr Penetration Tester  
**Focus:** Practical penetration testing, vulnerability assessment, exploitation, and security reporting

This repository contains personal notes, lab exercises, commands, and challenge write-ups documenting my progress through the learning path.

## 🛡️ Responsible Testing

All practical testing should be conducted within authorized labs or on systems for which explicit permission has been granted. The material is intended for educational purposes and responsible security practice.

---

### 🔐 Core Focus

**Reconnaissance • Nmap • Burp Suite • Web Application Security • OWASP Top 10 • Vulnerability Research • Metasploit • Privilege Escalation • Active Directory • Python • Pentesting Methodology and Reporting**
