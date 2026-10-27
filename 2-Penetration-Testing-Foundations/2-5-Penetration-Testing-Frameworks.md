# Jr.Pen 2.5. Penetration Testing Frameworks

### Task 1 - Introduction

Penetration testing frameworks provide a structured methodology to ensure tests are organized, stay in scope, cover all attack surfaces, and deliver actionable results without legal or operational risk. A penetration testing framework is a structured methodology that systematically guides testers through every phase of an engagement to ensure comprehensive, standardized security evaluations. 

Relying on a structured methodology provides several benefits. It ensures **thoroughness** so that critical areas are not overlooked. It promotes **consistency** so that different testers on the same team produce comparable results. It supports **compliance** by aligning the assessment with regulatory requirements. It also improves **communication** because clients, auditors, and stakeholders can understand and trust a process grounded in a recognized standard.

There are many penetration testing frameworks in active use today, and each has its own philosophy, strengths, and ideal use cases. In this room, we will explore the following in depth:

1. [Open Source Security Testing Methodology Manual (OSSTMM)](https://www.isecom.org/OSSTMM.3.pdf), a scientific, metrics-driven approach to security testing
2. [OWASP Web Security Testing Guide (WSTG)](https://owasp.org/www-project-web-security-testing-guide/), the go-to framework for web application assessments
3. [NIST Special Publication 800-115](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-115.pdf), the U.S. government's technical guide to security testing and assessment
4. [Penetration Testing Execution Standard (PTES)](http://www.pentest-standard.org/index.php/Main_Page), a practical, phase-driven standard that mirrors how real engagements are conducted
5. [Information Systems Security Assessment Framework (ISSAF)](https://untrustednetwork.net/files/issaf0.2.1.pdf), a historically influential methodology with a detailed nine-step assessment model

We will also introduce [MITRE ATT&CK](https://attack.mitre.org/) as a complementary knowledge base that maps adversary tactics and techniques. We will survey several other notable frameworks, including the [WASC Threat Classification](http://projects.webappsec.org/w/page/13246978/Threat%20Classification), [CSA Cloud Controls Matrix](https://cloudsecurityalliance.org/research/cloud-controls-matrix), [OWASP Mobile Application Security Testing Guide (MASTG)](https://mas.owasp.org/MASTG/), [PCI DSS Penetration Testing Guidelines](https://listings.pcisecuritystandards.org/documents/Penetration-Testing-Guidance-v1_1.pdf), and the [CBEST Framework](https://www.bankofengland.co.uk/financial-stability/operational-resilience-of-the-financial-sector/cbest-threat-intelligence-led-assessments-implementation-guide), so you know when and where they apply.

**Learning Objectives**

- Describe the purpose and structure of the major penetration testing frameworks.
- Compare frameworks based on their scope, methodology, and intended use cases.
- Select an appropriate framework for a given engagement scenario.
- Explain how MITRE ATT&CK complements traditional penetration testing methodologies.

**Answer the questions below**

> A penetration tester runs several tools against a target but skips network mapping entirely and does not document the scope beforehand. Which benefit of using a framework would have most directly prevented this situation?
> 

Thoroughness

> Your client operates in the healthcare sector and must demonstrate compliance with HIPAA. Beyond identifying vulnerabilities, which benefit of using a recognized framework would matter most to this client?
> 

Compliance

### Task 2 - OSSTMM

The Open Source Security Testing Methodology Manual (OSSTMM) uses a scientific approach to deliver quantifiable, verifiable, and repeatable security testing metrics rather than subjective judgments. OSSTMM organizes testing around five **security channels**, reflecting its philosophy that security is not just a network problem:

- **Human Security (HUMSEC)**: Social engineering and human-factor vulnerabilities.
- **Physical Security (PHYSSEC)**: Physical access controls, from badge readers to tailgating.
- **Wireless Communications (SPECSEC)**: Wi-Fi, Bluetooth, RFID, and other electromagnetic signals.
- **Telecommunications (COMSEC)**: Phone systems, VoIP, fax, and modem infrastructure.
- **Data Networks (DATASEC)**: Network services, firewalls, and application-layer protocols.

**OSSTMM's Five Channels:** Examining physical (PHYSSEC), human (HUMSEC), and technical vectors ensures holistic coverage, preventing strong controls in one area from being bypassed by oversights in another. 

**Risk Assessment Values (RAVs):** Quantitative metrics calculate the balance between total attack surface exposure and protective controls to yield objective, repeatable risk scores.

**Phases: A Walkthrough:** The OSSTMM testing cycle has four phases.  

**Phase 1: Induction -** Mapping and verifying active target assets to establish a confirmed inventory of the environment.

**Phase 2: Interaction -** Probing live assets to identify technologies, assess relevance, and quantify exposure for attack surface calculations.

**Phase 3: Inquiry** - Testing identified exposures to demonstrate unauthorized access and determine the precise scope of potential exploitation.

**Phase 4: Intervention -** Mitigating vulnerabilities through isolation and patching, auditing surrounding controls, and testing detection capabilities.

**Closing Notes:** While OSSTMM's STAR reporting and scientific approach provide auditable and repeatable results, its steep learning curve and resource intensity make it best suited for organizations prioritizing rigorous measurement.

**Answer the questions below**

> What OSSTMM metric quantifies the balance between an organization's attack surface and its controls?
> 

Risk Assessment Values

> During an OSSTMM assessment, you complete **Induction** and **Interaction**, identifying 6 hosts running unpatched services. Which phase do you enter next, and what is the primary objective?
> 

Inquiry

### Task 3 - OWASP WSTG

The OWASP Web Security Testing Guide (WSTG) provides a community-driven, risk-based framework organizing web application testing into over 90 discrete test cases across twelve categories. Categorized test cases prioritize vulnerabilities by exploitability and impact across the full web application attack surface.  

**Phases: Security Across the SDLC:** Unlike single-event frameworks, the WSTG integrates security testing across five distinct SDLC phases, from pre-development requirement setting through design, development, deployment, and ongoing maintenance.

**Phase 1: Before development begins.** Establishing security requirements and regulatory compliance standards before initial development begins. 

**Phase 2: During definition and design.** Conducting architectural reviews and threat modeling early in the design stage to build protective controls prior to coding. 

**Phase 3: During development.**  Reviewing source code and logic against WSTG test cases to identify and resolve vulnerabilities early in development.

**Phase 4: During deployment.** Performing penetration testing and verification in production-staged environments to ensure safe configurations and endpoint exposure controls. 

**Phase 5: During maintenance and operations.** Executing recurring health checks and targeted WSTG test cases following system updates to prevent new vulnerabilities post-launch.   

**Closing Notes:** While WSTG offers comprehensive and continually updated practical test procedures, its sheer scope can encourage a rigid checklist mindset or strain resource-limited teams requiring specialized expertise.

**Answer the questions below**

> The WSTG organizes its test cases using a specific identifier format. What identifier would you look for to find test cases related to input validation?
> 

WSTG-INPV

> A development team has just finished coding a new feature and wants to check it against WSTG before deployment. Which SDLC-aligned phase number are they in?
> 

3

### Task 4 - NIST SP 800-115

NIST SP 800-115 is a foundational framework published by NIST that provides structured, repeatable methodologies for technical security testing and assessment in federal and enterprise environments. Rather than focusing solely on penetration testing, the guide encompasses broader evaluation techniques to identify vulnerabilities, validate security controls, and assess exploitability. 

**Phases: A Walkthrough:** The framework structures security evaluations into systematic phases, progressing from low-impact passive reviews to active high-impact assessments.   

**Phase 1: Planning.** Establishing and documenting formal objectives, target scope, communication protocols, emergency stop conditions, and signed test plans before testing begins.

**Phase 2: Execution.** This is where active testing happens. NIST SP 800-115 groups execution activities into four technique categories:

- **Review techniques**: Examining documentation, policies, system configurations, firewall and rule sets. At GovNet, this includes reviewing  rules and access control lists for misconfigurations.
- **Target identification and analysis**: Discovering and fingerprinting live hosts, open ports, and running services. At GovNet, you scan the citizen portal's infrastructure and identify 12 internet-facing services.
- **Target vulnerability validation**: Confirming that identified weaknesses are real and exploitable, not false positives. At GovNet, a scan flags a potential SQL injection on the portal's search function; you manually validate it by crafting a test query that returns database version information.
- **Penetration testing**: Simulating adversarial attacks to test the depth of exploitation possible. At GovNet, you chain the confirmed SQL injection with a privilege escalation on the database server to demonstrate that an external attacker could access internal citizen records.

Notice how NIST SP 800-115 treats these as a progression from passive to active, from low-impact to high-impact. Not every engagement needs to reach the penetration testing stage; sometimes, a review and vulnerability validation are sufficient for the assessment's objectives.

**Phase 3: Post-Testing.** Analyzing results, mapping findings to bypassed controls, and delivering prioritized, actionable remediation guidance.

**Closing Notes:** While offering high credibility, standardization, and technology-agnostic flexibility across diverse environments, the framework acts as non-mandated guidance and requires highly skilled assessors capable of executing varied assessment techniques.   ****

**Answer the questions below**

> During the Execution phase, your vulnerability scanner flags 15 potential issues on a web application. Before attempting exploitation, which NIST SP 800-115 technique category should you apply to these findings?
> 

Target vulnerability validation  

### Task 5 - PTES

PTES provides an end-to-end, seven-phase framework that maps directly to the lifecycle of a penetration testing engagement, making it highly practical for structuring daily workflow. 

**Phases: A Walkthrough:** Let's walk through the seven phases using a scenario: a healthcare company, **MedGuard Health**, has hired your team to perform a full penetration test of their corporate network and patient records portal.

**Phase 1: Pre-Engagement Interactions.** Pre-Engagement Interactions: Focuses on pre-testing activities such as scoping, rules of engagement, and defining testing windows to prevent legal and professional disputes

**Phase 2: Intelligence Gathering.** Involves collecting target information using both passive and active techniques, where the depth of reconnaissance directly shapes subsequent phase quality. 

**Phase 3: Threat Modeling.** Uses gathered intelligence to identify valuable assets and likely attack paths, ensuring testing is guided by adversarial logic rather than random scanning. 

**Phase 4: Vulnerability Analysis.** Combines automated scanning and manual verification to systematically identify weaknesses while eliminating false positives.

**Phase 5: Exploitation.** Involves attempting to exploit confirmed vulnerabilities purposefully to demonstrate real-world business impact rather than merely gaining access. 

**Phase 6: Post-Exploitation.** Evaluates real-world impact after gaining access, successfully translating technical findings into tangible business risk.  

**Phase 7: Reporting.** Delivers findings through a structured dual-audience approach, providing an executive summary for leadership and technical details for IT remediation.   

**Closing Notes:** Offers a practical, end-to-end playbook ideal for building workflow instincts, though it requires supplementing tool-specific guidance due to a lack of recent updates.  

**Answer the questions below**

> Which PTES phase number specifically addresses defining the scope, rules of engagement, and legal authorization for a penetration test?
> 

1

### Task 6 - ISSAF

ISSAF is an open-source penetration testing framework developed by OISSG to evaluate network, system, and application security. It features a nine-step assessment model that mirrors attacker kill-chain progression, covering broad security domains through a risk-based approach that prioritizes high-impact vulnerabilities.

**Phases: A Walkthrough:** ISSAF divides an assessment into three phases. Let's walk through them with a scenario: your team is assessing the security of **TechBridge Solutions**, a software development company with 200 employees, an internal Git server, and a client-facing project management portal.

**Phase 1: Planning and Preparation:** This phase sets the engagement boundaries. You meet with TechBridge's CTO to define the scope (corporate network, Git server, and the project management portal), establish escalation protocols and emergency contacts, identify constraints (the production Git server must not be disrupted during business hours), and agree on the toolset appropriate for the assessment.

**Phase 2: Assessment:** This is the core of ISSAF and where its nine-step model lives. Each step builds on the previous one, simulating how a real adversary would progress through the environment.

1. **Information gathering**: Collect publicly available data about TechBridge. DNS records, WHOIS data, employee profiles on LinkedIn, and technology references in job postings ("experience with Jenkins and GitLab CI required") all feed your understanding of the target.
2. **Network mapping**: Map the live network topology. You discover TechBridge's external IP range hosts the project portal, a VPN gateway, and a mail server. Internal scanning (once in scope) reveals the Git server, a Jenkins build server, and several developer workstations.
3. **Vulnerability identification**: Scan the mapped assets for weaknesses. The project portal runs an outdated CMS with a known authentication bypass. The Jenkins server has its administrative console exposed without authentication.
4. **Penetration**: Attempt initial exploitation. You exploit the unauthenticated Jenkins console to execute system commands on the build server.
5. **Gaining access and privilege escalation**: Escalate from initial access to higher privileges. From the Jenkins server, you recover stored credentials for the service account that deploys code to production, which has administrative rights on the Git server.
6. **Enumerating further**: With elevated access, enumerate what is now reachable. From the Git server, you discover repositories containing API keys, database connection strings, and client project source code.
7. **Compromise remote users/sites (lateral movement)**: Move laterally to other systems. Using the harvested credentials, you access several developer workstations and the internal mail server.
8. **Maintaining access**: Establish persistent access to demonstrate that a real attacker could retain their foothold. You document (without actually deploying) how a backdoor could be planted in the CI/CD pipeline, persisting across system reboots and deployments.
9. **Covering tracks**: Demonstrate how an attacker would erase evidence. You document which logs captured your activity and identify gaps in TechBridge's logging that would allow a real adversary to operate undetected.

Notice the progression: each step deepens the attacker's position in the environment. Steps 1 through 3 are reconnaissance and analysis, steps 4 through 7 are active compromise, and steps 8 through 9 address persistence and stealth.

**Phase 3: Reporting and Cleanup:** You compile findings into a structured report, prioritized by business impact. The unauthenticated Jenkins console is flagged as critical because it provided the initial foothold that led to source code access. Cleanup involves removing any test artifacts, revoking any temporary accounts created during testing, and confirming with TechBridge's team that no testing residue remains in their environment.

**Closing Notes:** Offering a valuable nine-step educational model that mirrors real-world attack progressions, though its obsolete technical procedures require supplementation from current frameworks like PTES or OWASP WSTG.

**Answer the questions below**

> ISSAF's nine-step assessment model begins with information gathering. What is the ninth and final step?
> 

Covering tracks

### Task 7 - MITRE ATT&CK

ATT&CK serves as a knowledge base of real-world adversary behavior that addresses the gap left by traditional penetration testing frameworks by helping organizations compare their exposure to active threat actors in the wild.

**The Matrix (Tactics):** The framework is organized as a matrix where columns represent high-level adversary objectives known as tactics (the "why"), spanning 14 enterprise categories ranging from initial access to impact

**Techniques and Sub-Techniques:** Within each tactic column, specific methods ("how" actions are performed) are detailed as techniques and sub-techniques, complete with descriptions, real-world threat group examples, detection recommendations, and mitigations

**Complementary Role:** Rather than prescribing how to run a penetration test, ATT&CK acts as a standardized reference dictionary to categorize and describe adversary behavior observed during assessments.  

**Walkthrough: Mapping Findings to ATT&CK -** Let's see how this works in practice. Here is how the key findings from that engagement map to ATT&CK technique IDs:

| **Engagement Finding** | **ATT&CK Tactic** | **ATT&CK Technique** |
| --- | --- | --- |
| Phishing email delivered payload to employee workstation | Initial Access | Phishing: Spearphishing Attachment (`T1566.001`) |
| Exploited Tomcat deserialization flaw on patient portal | Initial Access | Exploit Public-Facing Application (`T1190`) |
| Extracted cached domain credentials from workstation | Credential Access | OS Credential Dumping (`T1003`) |
| Moved from workstation to file server using stolen credentials | Lateral Movement | Use Alternate Authentication Material (`T1550`) |
| Accessed patient records database from compromised portal server | Collection | Data from Information Repositories (`T1213`) |

**Closing Notes:** MITRE ATT&CK acts as a standardized universal translator that complements penetration testing frameworks by describing findings through real-world adversary behavior.

**Answer the questions below**

> In the ATT&CK matrix, what do the columns represent?
> 

tactics

> In the ATT&CK matrix, what do the rows within each column represent?
> 

techniques

> You compromised a web server by exploiting an unpatched vulnerability in its public-facing application. What ATT&CK technique ID would you use to classify this finding?
> 

T1190

### Task 8 - Other Notable Frameworks

Beyond core methodologies, specialized frameworks provide landscape awareness of industry-specific, platform, and regulatory requirements for targeted client engagements. 

**The Frameworks**

- **WASC Threat Classification:** Developed by WASC as a taxonomy for categorizing web application vulnerabilities and attack types, though it has been largely superseded by the OWASP Top Ten and WSTG.
- **CSA Cloud Controls Matrix (CCM):** Published by the Cloud Security Alliance as a governance and compliance framework for cloud computing environments to evaluate cloud provider and configuration security postures.
- **OWASP Mobile Application Security Testing Guide (MASTG):** Serves as the mobile counterpart to the WSTG, providing detailed Android and iOS security test cases used alongside the MASVS.
- **PCI DSS Penetration Testing Guidelines:** Mandates regulatory penetration testing requirements under Requirement 11.4 for organizations handling cardholder data, including annual testing, post-change assessments, and network segmentation validation.
- **CBEST Framework:** Developed by the Bank of England for UK financial institutions to integrate custom threat intelligence with realistic adversary simulations under regulatory backing.

**Comparison Table**

| **Framework** | **Primary Domain** | **Type** | **Actively Maintained** | **When You Would Use It** |
| --- | --- | --- | --- | --- |
| WASC Threat Classification | Web applications | Threat taxonomy | No (superseded by OWASP) | Legacy references; historical context |
| CSA Cloud Control Matrix | Cloud environments | Governance/compliance controls | Yes | Cloud security posture assessments |
| OWASP Mobile Application Security Testing Guide | Mobile applications (Android/iOS) | Testing guide | Yes | Mobile app penetration testing |
| PCI DSS Penetration Testing Guidelines | Payment card environments | Regulatory mandate | Yes (current: PCI DSS v4.0) | Any engagement involving cardholder data |
| CBEST | UK financial sector | Threat-intel-led pen testing | Yes | Engagements with UK financial institutions |

**Closing Notes:** Framework selection is driven by contextual factors such as the client's industry, regulatory obligations, technology platform, and geographic jurisdiction.   

**Answer the questions below**

> Your client is a European online retailer that processes credit card payments through their website. Which framework from this task would most directly govern the penetration testing requirements for this engagement?
> 

PCI DSS Penetration Testing Guidelines

> A client asks you to assess the security of their iOS banking application, including how it stores credentials locally and communicates with backend APIs. Which framework from this task is the most appropriate testing reference?
> 

OWASP Mobile Application Security Testing Guide

> Your team is evaluating a client's AWS infrastructure to determine whether their cloud configurations meet industry security standards. They are not asking for a penetration test but rather a controls assessment. Which framework is most relevant?
> 

CSA Cloud Control Matrix

### Task 9 - Choosing the Right Framework

Framework selection is a critical initial decision guided by multiple factors, including engagement scope, regulatory mandates, quantitative requirements, and team expertise.

**Selection Criteria:** Four factors consistently drive framework selection in real-world engagements.

**Engagement scope and target type** The immediate filter that aligns technical targets (web apps, mobile apps, or full networks) with specialized frameworks like OWASP WSTG, MASTG, or PTES.

**Regulatory and compliance requirements** Legal and industry mandates (such as PCI DSS, CBEST, or NIST SP 800-115) override personal preferences and dictate specific assessment rules.

**Quantifiable & Repeatable Results:** OSSTMM's RAV metrics provide objective measurements necessary for cross-team and year-over-year security posture comparisons.

**Team expertise and available resources** Practical constraints that balance methodological complexity and resource overhead against the team's capabilities.

**Scenario Practice:** Let's apply these criteria to realistic situations. For each scenario, consider which framework (or combination) you would recommend and why.

**Scenario 1 (Hospital Assessment):** Combining PTES, OWASP WSTG, and MITRE ATT&CK to deliver a structured end-to-end evaluation of a hospital's network and web portal under HIPAA guidelines.

**Scenario 2 (Multinational Bank):** Utilizing CBEST to satisfy UK financial regulatory obligations and integrate bespoke threat intelligence.

**Scenario 3 (SaaS Startup):** Leveraging OSSTMM's quantitative RAV metrics alongside OWASP WSTG to enable objective year-over-year security tracking.

**Scenario 4 (Fintech Mobile Apps):** Dual-framework approach using OWASP MASTG for mobile security testing alongside mandatory PCI DSS guidelines for cardholder data handling.

**Closing Notes:** Real-world penetration testing typically involves combining a primary framework with complementary methodologies to address overlapping technical, regulatory, and reporting requirements.

**Answer the questions below**

> A U.S. federal agency needs a security assessment of its internal network. The results must align with federal guidelines, and the assessment should include both document reviews and active penetration testing. Which framework is the most natural primary choice?
> 

NIST SP 800-115

> Your client is an e-commerce company with a web storefront, a mobile shopping app, and a payment processing system. Which combination of frameworks would you recommend to cover all three components? (comma-separrated)
> 

WSTG,MASTG,PCI DSS

### Task 10 - Conclusion

**Master Comparison Table**

| Framework | Scope | Key Strength | Limitation | Best For |
| --- | --- | --- | --- | --- |
| OSSTMM | Multi-channel (5 channels) | Quantifiable metrics (RAVs) | Steep learning curve | Repeatable, auditable assessments |
| OWASP WSTG | Web applications | 90+ structured test cases | Can encourage checklist mentality | Web app penetration testing |
| NIST SP 800-115 | Systems, networks, applications | Federal credibility; flexible technique categories | Guidance only, no enforcement | Government and enterprise assessments |
| PTES | End-to-end engagements | Practical, phase-driven workflow | Not recently updated | Standard network/application pentests |
| ISSAF | Networks, systems, applications | Clear nine-step adversarial progression | No longer maintained | Educational reference for attack methodology |
| MITRE ATT&CK | Adversary behavior catalog | Universal language for threat behavior | Not a testing methodology | Enriching findings from any framework |
| OWASP MSTG | Mobile applications (Android/iOS) | Platform-specific mobile test cases | Mobile-only scope | Mobile app security testing |
| PCI DSS Guidelines | Payment card environments | Regulatory mandate | Narrow applicability | Engagements involving cardholder data |
| CSA CCM | Cloud environments | Governance controls mapped to major standards | Not a penetration testing methodology | Cloud security posture assessments |
| CBEST | UK financial sector | Threat-intel-led, regulatory backing | UK financial sector only | UK financial institution engagements |
| WASC Threat Classification | Web applications | Historical taxonomy | Superseded by OWASP | Legacy reference only |

**Answer the questions below**

> Go through the attached static site. What is the flag?
> 

THM{pen-test-fr4m3work5}

<img width="601" height="555" alt="image" src="https://github.com/user-attachments/assets/a35b16f2-0707-472f-a2ad-e6b4fa5b17a1" />
<img width="601" height="484" alt="image 1" src="https://github.com/user-attachments/assets/e09dd735-e6be-48de-b59c-0270f7416aa2" />
<img width="601" height="532" alt="image 2" src="https://github.com/user-attachments/assets/95ef579b-2b19-46f3-bb12-fb56a831e51d" />
<img width="601" height="532" alt="image 3" src="https://github.com/user-attachments/assets/e95c5e97-b868-451f-8f8a-47eaf9f446dd" />
<img width="601" height="532" alt="image 4" src="https://github.com/user-attachments/assets/d3b16bd3-2016-47cc-aafc-9ec771d4a704" />

  
