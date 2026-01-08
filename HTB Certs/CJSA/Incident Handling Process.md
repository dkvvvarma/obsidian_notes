
Incident Handling(IH) has become a crucial part of organisation's defence against cybercrime.

An `event` is an action occurring in a system or network. Examples of events include:

- A user sending an email.
- A mouse click.
- A firewall allowing a connection request.
  
An incident is an event with negative consequence.(Example - System Crash, Unauthorised access to sensitive data or natural disasters.).
- Data theft.
- Funds theft.
- Unauthorized access to data.
- Installation and use of malware and remote access tools.

`Incident handling is a clearly defined set of procedures for managing and responding to security incidents in a computer or network environment.`

![[Pasted image 20260102234520.png]]

Other types of incidents, such as those caused by malicious insiders, availability issues, and loss of intellectual property, also fall within the scope of incident handling. A comprehensive incident handling plan should address various types of incidents and provide appropriate measures to identify, contain, eradicate, and recover from them to restore normal business operations as quickly and efficiently as possible.

The incident handling team is led by an incident manager. This role is often assigned to a SOC manager, CISO/CIO, or third-party (trusted) vendor, and this person usually has the ability to direct other business units as well. The incident manager must be able to obtain information or have the mandate to require any employee in the organization to perform an activity in a timely manner, if necessary. The incident manager is the single point of communication who tracks the activities taken during the investigation and their status of completion.

### Different Types of Real-World Incidents

### Leaked Credentials
- `Colonial Pipeline Ransomware Attack`: The Colonial Pipeline, a major American oil pipeline system, fell victim to a ransomware attack. This [attack](https://en.wikipedia.org/wiki/Colonial_Pipeline_ransomware_attack) originated from a breached employee's personal password, likely found on the dark web, rather than a direct attack on the company's network. The attackers gained access to the company's systems using a compromised password for an inactive VPN (Virtual Private Network) account, which did not have Multi-Factor Authentication (MFA) enabled.

### Default / Weak Credentials
- `Mirai Botnet (2016)`: The Mirai botnet scanned for IoT devices using factory or default credentials (e.g., admin/admin) and conscripted them into a massive DDoS botnet. This led to large-scale DDoS disruptions affecting companies like Dyn and OVH, with hundreds of thousands of devices infected. The root cause was the devices being shipped with unchanged default credentials and poor remote access security.
- `LogicMonitor Incident (2023)`: Some LogicMonitor customers were compromised because the vendor issued weak default passwords to customer accounts. Affected customers experienced follow-on ransomware incidents or unauthorized access. The root cause involved vendor-assigned weak/default credentials and delayed enforcement of password hardening.

### Outdated Software / Unpatched Systems
- `Equifax (2017) Breach`: Attackers exploited a known Apache Struts vulnerability (CVE-2017-5638) in Equifax’s web application. This breach exposed the personal data of approximately 143–147 million people, leading to major regulatory and legal fallout. The incident occurred due to a failure to apply a publicly released patch in a timely manner.
- `WannaCry (2017)`: The WannaCry ransomware spread as a worm using the SMB EternalBlue exploit, affecting more than 200,000 systems across over 150 countries. High-profile impacts included hospitals and enterprises. This incident was due to unpatched Windows systems, despite the MS17-010 patch being available before the outbreak.

### Rogue Employee / Insider Threat
- `Cash App / Block Inc. (2021 Disclosure; Public 2022 Notice)`: A former employee accessed the personal information of millions of Cash App users, as reported in company disclosures. Approximately 8.2 million current and former customers were potentially impacted, leading to regulatory scrutiny and settlements. The root cause was the abuse of legitimate employee access and insufficient internal controls and monitoring.

### Phishing / Social Engineering
- `Industry Trend & Representative Data`: Phishing is a pervasive vector used to obtain credentials, deliver malware, or trick users into enabling remote access. It frequently leads to account compromise, fraud, and network footholds. A significant portion of breaches over multiple years are linked to phishing.
- `U.S. Interior Department Phishing Attack`: Attackers used an "evil twin" technique to trick individuals into connecting to a fake Wi-Fi network, allowing hackers to steal credentials and access the network. This incident revealed a lack of secure wireless network infrastructure and insufficient security measures, including weak user authentication and inadequate network testing.
- `2020 Twitter Account Hijacking`: In 2020, many high-profile Twitter accounts were compromised by outside parties to promote a bitcoin scam. Attackers gained access to Twitter's administrative tools, allowing them to alter accounts and post tweets directly. They appeared to have used social engineering to gain access to the tools via Twitter employees.

### Supply-Chain Attack
- `SolarWinds Orion (2020)`: Nation-state actors compromised the SolarWinds build/release environment and injected a malicious backdoor into Orion updates, which were distributed to thousands of customers. This caused wide-reaching espionage and unauthorized access across government and private sectors, leading to protracted detection and remediation efforts.
  
An example of an incident report from DFIR Labs is as follows:
- [Confluence Exploit Leads to LockBit Ransomware](https://thedfirreport.com/2025/02/24/confluence-exploit-leads-to-lockbit-ransomware/)
  
Here's another example of an incident report from Cybereason.
- [CHAES:Novel Malware Targeting Latin American E-Commerce](https://www.cybereason.com/hubfs/dam/collateral/reports/11-2020-Chaes-e-commerce-malware-research.pdf)

A report from PaloAlto Unit42 that covers global incidents is as follows:
- [Global Incident Response Report](https://www.paloaltonetworks.com/engage/unit42-2025-global-incident-response-report)
  
### Incident Scenario
Throughout this module, we'll refer to an incident scenario to understand some challenges that incident handlers face. This incident shows an example of the patterns repeatedly observed in real-world incidents. The victim in this scenario is `Insight Nexus`, a global market research firm that handles sensitive competitive data for high-profile clients in the IT sector. The firm becomes a target of two distinct threat groups operating simultaneously within its environment.

![[Pasted image 20260104165152.png]]

Based on the information we have collected, the first threat actor gained entry when system administrators forgot to change the default admin/admin password on an internet-facing application, i.e., ManageEngine ADManager Plus, after a product update. By leveraging this, the attackers logged in successfully, performed reconnaissance, mapped users and machines, and eventually created new privileged Active Directory accounts. Using one of the newly created accounts, the adversaries pivoted further into the environment, identifying an external RDP service exposed by misconfiguration. Exploiting that entry point, they escalated their control and eventually used Group Policy Objects (GPOs) to deploy spyware using an MSI package across multiple endpoints.

GPO - Group policy objects are centralized configuration containers in Microsoft Active Directory that enforce security, system and user behaviours across Windows env at large scale.

GPOs let an organization:

- Enforce **security baselines**
- Control **user behavior**
- Standardize **system configuration*
- Reduce **manual administration**
- Minimize **attack surface**
All from one control plane.

#### Where GPOs live
- Stored in:
    - **Active Directory (AD)**
    - **SYSVOL** (replicated file share)

- Managed via
    - **Group Policy Management Console (GPMC)**

 Scope: who GPOs apply to
GPOs are linked to:
- Sites
- Domains
- Organizational Units (OUs)

And apply to:
- Users
- Computers

#### What GPOs can control (real examples
#### Security & access control
- Password complexity and length
- Account lockout thresholds
- UAC behavior
- Windows Defender rules
- Firewall rules
- SMB signing
- NTLM restrictions

##### User environment
- Disable Control Panel
- Block USB storage
- Map network drives
- Set desktop wallpaper
- Restrict PowerShell

##### System hardening
- Disable legacy protocols (SMBv1)
- Enforce BitLocker
- Credential Guard / LSA protection
- RDP settings
- Audit policies


##### Software & scripts
- Startup / shutdown scripts    
- Logon / logoff scripts   
- Software installation (MSI)
- Scheduled tasks

### Why GPOs matter in security (tell-it-like-it-is)
#### For defenders
- **One misconfigured GPO = domain-wide exposure**
- Strong GPOs dramatically reduce:
    - Lateral movement
    - Credential theft
    - Malware persistence

### For attackers
GPOs are **high-value targets** because:
- Editing a GPO = code execution on every machine it applies to
- GPO abuse = stealthy persistence
- SYSVOL write access = game over

This is why:
- GPO permissions are **Tier 0 assets**
- Red teams hunt them aggressively

### Common GPO misconfigurations (real-world pain points)
- Weak ACLs on GPOs
- Authenticated Users with edit rights
- Scripts stored in SYSVOL with write access
- GPOs running PowerShell as SYSTEM
- No auditing on GPO changes
These are **enterprise-killers**, not edge cases.

### How GPOs are enforced technically
- Client polls AD every:
    - ~90 minutes (workstations)
    - ~5 minutes (domain controllers)
- Uses:
    - LDAP
    - SMB (SYSVOL)

- Applies settings locally
- Stores results in:
    - Registry
    - Local security database
    - Scheduled tasks

## Cyber Kill Chain
A security framework developed by Lockheed Martin cyber in 2011 to identify and mitigate security incidents and by breaking down a cyberattack into distinct stages. (Inspired by military kill chain strategies it models progression of attack to help security teams detect,prevent and respond to threats at each phase. Particularly effective in analyzing APT)

![[Pasted image 20260104171302.png]]
### Stages of Cyber Kill Chain

`Reconnaissance` the initial stage where attacker scopes and chooses the target. This stages where attacker gathers info to become familiar with target.Some attackers prefer to perform passive information gathering from web sources such as LinkedIn and Instagram, but also from documentation on the target organisation's web pages. Job ads and company partners often reveal information about the technology utilised in the target organization. They can provide extremely specific information about antivirus tools, operating systems, and networking technologies. Other attackers go a step further; they start 'poking' and actively scan external web applications and IP addresses that belong to the target organization.

![Reconnaissance Stage diagram split into Active and Passive Recon. Active Recon: Identify target and scope; Locate open ports; Identify services on open ports; Map entire network. Passive Recon: Information gathering from web sources (job ads, company partners); Social media (LinkedIn, Instagram, Facebook); Avoid detection at all times. Icons accompany each item on a dark background.](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/148/ir_recon.png)

In the `Weaponize` stage,malware is used for initial access and embedded into some type of exploit/Trojan. malware crafted will extremely lightweight and undetectable by anti virus and detection tools. likely that the attacker has gathered information to identify the present antivirus or EDR technology present in the target organization also employ techniques to have persistent access.

In `Delivery` stage the exploit or payload is delivered to victim. Traditional approach include phishing mails that contain a malicious attachment to link or web page.(Two ways (1) Web page contains exploit  to avoid email scanners (2) Web page mimics a legitimate site into tricking the user enter credentials.The payload in these trust-gaining cases is hosted on an attacker-controlled website that mimics a well-known website to the victim (e.g., a copy of the target organization's website). It is extremely rare to deliver a payload that requires the victim to do more than double-click an executable file or a script (in Windows environments, this can be .bat, .cmd, .vbs, .js, .hta, and other formats). Finally, there are cases where physical interaction is utilized to deliver the payload via USB tokens and similar storage tools that are purposely left around.

The `Exploitation` stage is moment when an exploit or delivered payload is triggered. During exploitation stage of Cyber Kill Chain, the attacker attempts to execute code on target system in order to gain access or control.

In `Installation` stage the initial stage is executed and running on compromised machine.There are installation can be carried out in various ways depending on attacker's goal and nature of compromise.Some common techniques used in the installation stage include:

- **Droppers**: Attackers may use droppers to deliver malware onto the target system. A dropper is a small piece of code designed to install malware on the system and execute it. The dropper may be delivered through various means, such as email attachments, malicious websites, or social engineering tactics.

- **Backdoors**: A backdoor is a type of malware designed to provide the attacker with ongoing access to the compromised system. The backdoor may be installed by the attacker during the exploitation stage or delivered through a dropper. Once installed, the backdoor can be used to execute further attacks or steal data from the compromised system.

- **Rootkits**: A rootkit is a type of malware designed to hide its presence on a compromised system. Rootkits are often used in the installation stage to evade detection by antivirus software and other security tools. The rootkit may be installed by the attacker during the exploitation stage or delivered through a dropper.

In the `Command and Control` stage, the attacker establishes a remote access capability to the compromised machine. As discussed, it is not uncommon to use a modular initial stager that loads additional scripts 'on-the-fly'. However, advanced groups will utilize separate tools to ensure that multiple variants of their malware live in a compromised network, and if one of them gets discovered and contained, they still have the means to return to the environment.

The final stage of the chain is the `Action` or objective of the attack. The objective of each attack can vary. Some adversaries may aim to exfiltrate confidential data, while others may want to obtain the highest level of access possible within a network to deploy ransomware. Ransomware is a type of malware that renders all data stored on endpoint devices and servers unusable or inaccessible unless a ransom is paid within a limited time frame (not recommended).

Incident Responder objective is to `stop an attacker from progressing further up the kill chain`, ideally in one of the earliest stages.

### MITRE ATT&CK Framework

Another framework for understanding adversary behaviour is the [MITRE ATT&CK](https://attack.mitre.org/) framework. It is a more granular, matrix-based knowledge base of adversary tactics and techniques used to achieve specific goals. Cybersecurity professionals use both frameworks to understand and defend against cyberattacks.

The MITRE ATT&CK Enterprise Matrix is a knowledge base that documents adversary behavior observed in the wild against enterprise IT environments (Windows, Linux, macOS, cloud, network, mobile, etc.). It is presented as a `matrix` where columns represent adversary goals (`tactics`), and cells are `techniques` attackers use to achieve those goals. The framework helps defenders understand, model, detect, and respond to attacker behavior in a structured way.

#### Tactic
A tactic is a high-level adversary objective during an intrusion (the goal they want to accomplish at that stage). For Example:

- `Initial Access`.
- `Persistence`.
- `Privilege Escalation`.

#### Technique
A technique is a specific method adversaries use to achieve a tactic. Techniques describe concrete attacker behavior (tools, commands, APIs, protocols, etc.).

Techniques have IDs like [T1105 (Ingress Tool Transfer)](https://attack.mitre.org/techniques/T1105/) or [T1021 (Remote Services)](https://attack.mitre.org/techniques/T1021/). For example:

- `T1105 Ingress Tool Transfer`: Refers to the tools used by attackers to download a tool, such as `wget`, `curl`, etc., commonly OS built-in commands/tools.
- `T1021 Remote Services`: Refers to adversaries using protocols such as SSH, RDP, and SMB for lateral movement.

#### Sub-technique
Sub-techniques are children of techniques that capture a particular implementation or target. Sub-technique IDs extend the parent technique: [T1003.001 (Credential Dumping -> LSASS Memory)](https://attack.mitre.org/techniques/T1003/001/), [T1021.002 (Remote Services -> SMB/Windows Admin Shares)](https://attack.mitre.org/techniques/T1021/002/). For example:

- `T1003.001 - OS Credentials: LSASS Memory`: Refers to adversaries dumping credentials directly from the LSASS process memory when achieving the necessary privileges.
- `T1021.002 - Remote Services: SMB/Windows Admin Shares`: Refers to adversaries interacting with shares using valid credentials.

This enables precise detection, attribution, and reporting (we can say "We detected `T1003.001` — LSASS memory dumping" instead of just `T1003`).

### Pyramid of Pain
In the diagram below, the Pyramid of Pain illustrates how much `effort it takes for an adversary to change their tactics` when defenders detect and block different types of indicators. At the base of the pyramid are simple indicators like hash values, IP addresses, and domain names — these are easily changed by attackers (low pain).

![[Pasted image 20260106204901.png]]In summary:

- Hash/IP detections = `easy` to evade.
- Behavioral TTP detections (MITRE-based) = `hard to evade`, higher attacker cost, and stronger defense maturity.
  
### Example of MITRE ATT&CK Mapping
The table below shows some of the techniques (MITRE ATT&CK) that were observed during the incident.

|Tactic|Technique|ID|Description|
|---|---|---|---|
|`Initial Access`|Exploit Public-Facing Application|T1190|Confluence CVE exploited|
|`Execution`|Command and Scripting Interpreter: PowerShell|T1059.001|PowerShell used for payload download|
|`Persistence`|Windows Service|T1543.003|Windows Service for persistence|
|`Credential Access`|LSASS Memory Dumping|T1003.001|Extracted credentials|
|`Lateral Movement`|Remote Desktop Protocol|T1021.001|RDP lateral movement|
|`Impact`|Data Encrypted for Impact|T1486|LockBit ransomware|

### Incident Handling Process Overview
The `Incident Handling Process` defines a capability for organizations to prepare, detect, and respond to malicious events. Note that this process is suited for responding to IT security events, but its stages do not correspond to the stages of the Cyber Kill Chain in a one-to-one manner.

![[Pasted image 20260106210403.png]]

So, incident handling has two main activities, which are `investigating` and `recovering`. The investigation aims to:

- `Discover` the initial '`patient zero`' victim and create an ongoing (if still active) incident timeline.
- Determine which `tools` and malware the adversary used.
- `Document` the compromised systems and what the adversary has done.

Following the investigation, the recovery activity involves `creating and implementing a recovery plan`. Once the plan is implemented, the business should resume normal operations, if the incident caused any disruptions.

## Preparation Stage (Part 1)
In the `Preparation` stage, we have two separate objectives. The first is the establishment of incident handling capability within the organization. The second is the ability to protect against and prevent IT security incidents by implementing appropriate protective measures. Such measures include endpoint and server hardening, Active Directory tiering, Multi-Factor Authentication, privileged access management, and so on. While protecting against incidents is not the responsibility of the incident handling team, this activity is fundamental to the overall success of that team.

### Preparation Prerequisites
During preparation stage, we need to ensure that we have
- Skilled incident handling team members (incident handling team members can be outsourced, but a basic capability and understanding of incident handling are necessary in-house regardless).
- A trained workforce (as much as possible, through security awareness activities or other means of training).
- Clear policies and documentation.
- Tools (software and hardware).

### Clear Policies & Documentation
Some of the written policies and documentation should contain an up-to-date version of the following information:
- Contact information and roles of the incident handling team members.
- Contact information for the legal and compliance department, management team, IT support, communications and media relations department, law enforcement, internet service providers, facility management, and external incident response team.
- Incident response policy, plan, and procedures.
- Incident information sharing policy and procedures.
- Baselines of systems and networks, out of a golden image and a clean state environment.
- Network diagrams.
- Organisation-wide asset management database.
- User accounts with excessive privileges that can be used on-demand by the team when necessary (also for business-critical systems, which are handled with the skills needed to administer that specific system). These user accounts are normally enabled when an incident is confirmed during the initial investigation and then disabled once it is over. A mandatory password reset is also performed when disabling the users.
- Ability to acquire hardware, software, or an external resource without a complete procurement process (urgent purchase of up to a certain amount). The last thing you need during an incident is to wait for weeks for the approval of a $500 tool.
- Forensic/Investigative cheat sheets.

### Tools (Software & Hardware)
Moving forward, we also need to ensure that we have the right tools to perform the job. These include, but are not limited to:

- An additional laptop or a forensic workstation for each incident handling team member to preserve disk images and log files, perform data analysis, and investigate without any restrictions (we know malware will be tested here, so tools such as antivirus should be disabled). These devices should be handled appropriately and not in a way that introduces risks to the organization.
- Digital forensic image acquisition and analysis tools.
- Memory capture and analysis tools.
- Live response capture and analysis tools.
- Log analysis tools.
- Network capture and analysis tools.
- Network cables and switches.
- Write blockers.
- Hard drives for forensic imaging.
- Power cables.
- Screwdrivers, tweezers, and other relevant tools to repair or disassemble hardware devices if needed.
- Indicator of Compromise (IOC) creator and the ability to search for IOCs across the organization.
- Chain of custody forms.
- Encryption software.
- Ticket tracking system.
- Secure facility for storage and investigation.
  
Many of the tools mentioned above will be part of what is known as a `jump bag` - always ready with the necessary tools to be picked up and taken immediately. Without this prepared bag, gathering all necessary tools on the fly may take days or weeks before we are ready to respond.

## Preparation Stage (Part 2)
Another part of the `Preparation` stage is to protect against incidents. While protection is not necessarily the responsibility of the incident handling team, any protection-related activities should be known to them to better understand the type and sophistication of an incident and know where to look for artifacts or evidence, that could aid the investigation.

### DMARC
[DMARC](https://dmarcly.com/blog/how-to-implement-dmarc-dkim-spf-to-stop-email-spoofing-phishing-the-definitive-guide#what-is-dmarc) is an email protection mechanism against phishing built on top of the already existing [SPF](https://dmarcly.com/blog/how-to-implement-dmarc-dkim-spf-to-stop-email-spoofing-phishing-the-definitive-guide#what-is-spf) and [DKIM](https://dmarcly.com/blog/how-to-implement-dmarc-dkim-spf-to-stop-email-spoofing-phishing-the-definitive-guide#what-is-dkim).

## Detection & Analysis Stage (Part 1) 

The detection & Analysis stage involves all aspects of detecting an incident such as utilising sensors,logs and trained personnel.egmentation of the architecture and having a clear understanding of and visibility within the network are also important factors.

Threats are introduced to the organization via an infinite number of attack vectors, and their detection can come from sources such as:
- An employee who notices abnormal behavior.
- An alert from one of our tools (EDR, IDS, Firewall, SIEM, etc.).
- Threat hunting activities.
- A third-party notification informing us that they discovered signs of our organization being compromised.

It is highly recommended to create levels of detection by logically categorizing our network as follows:
- Detection at the network perimeter (using firewalls, internet-facing network intrusion detection/prevention systems, demilitarized zone, etc.).
- Detection at the internal network level (using local firewalls, host intrusion detection/prevention systems, etc.).
- Detection at the endpoint level (using antivirus systems, endpoint detection & response systems, etc.).
- Detection at the application level (using application logs, service logs, etc.).
  
### Initial investigation
Think about how information is presented in the event of an administrative account connecting to an IP address at HH:MM:SS. Without knowing what system is on that IP address and which time zone the time refers to, we may easily jump to the wrong conclusion about what this event is about. To sum up, we should aim to collect as much information as possible at this stage about the following:

- Date/Time when the incident was reported. Additionally, who detected the incident and/or who reported it?
- How was the incident detected?
- What was the incident? Phishing? System unavailability? etc.
- Assemble a list of impacted systems (if relevant).
- Document who has accessed the impacted systems and what actions have been taken. Make a note of whether this is an ongoing incident or if the suspicious activity has been stopped.
- Physical location, operating systems, IP addresses and hostnames, system owner, system's purpose, current state of the system.
- List of IP addresses, if malware is involved, time and date of detection, type of malware, systems impacted, export of malicious files with forensic information on them (such as hashes, copies of the files, etc.).

### Incident Severity & Extent Questions

When handling a security incident, we should also try to answer the following questions to get an idea of the incident's severity and extent:

- What is the exploitation impact?
- What are the exploitation requirements?
- Can any business-critical systems be affected by the incident?
- Are there any suggested remediation steps?
- How many systems have been impacted?
- Is the exploit being used in the wild?
- Does the exploit have any worm-like capabilities?

The last two can possibly indicate the level of sophistication of an adversary.

### Incident Confidentiality & Communication
Incidents are very confidential topics, and as such, all of the information gathered should be kept on a need-to-know basis unless applicable laws or a management decision instruct us otherwise. There are multiple reasons for this. The adversary may be, for example, an employee of the company, or if a breach has occurred, the communication to internal and external parties should be handled by the appointed person in accordance with the legal department.

## Detection & Analysis Stage(Part 2)
When an investigation is started, we aim to understand `what happened` and `how it happened`

#### The Investigation
The investigation starts based on the initially gathered (and limited) information that contains what we know about the incident so far. With this initial data, we will begin a 3-step cyclic process that will iterate over and over again as the investigation evolves. This process includes:

- Creation and usage of indicators of compromise (IOCs).
- Identification of new leads and impacted systems.
- Data collection and analysis from the new leads and impacted systems.

![Flowchart showing investigation process: Initial Investigation Data leads to IOCs, Compromised Systems, and Collection & Analysis.](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/148/ir-ioc.png)

### Initial Investigation Data
In order to reach a conclusion, an investigation should be based on valid leads that have been discovered not only during this initial phase but throughout the entire investigation process. The incident handling team should constantly bring up new leads and not focus solely on a specific finding, such as a known malicious tool.

### Creation & Usage Of IOCs
An indicator of compromise (IOC) is a `sign that an incident has occurred`. IOCs are documented in a structured manner, which represents the `artifacts` of the compromise. Examples of IOCs can be IP addresses, hash values of files, and file names. In fact, because IOCs are so important to an investigation, special languages such as `OpenIOC` have been developed to document them and share them in a standard manner. Another widely used standard for IOCs is `YARA`. There are a number of free tools that can be utilized, such as Mandiant's `IOC Editor`, to create or edit IOCs. Using these languages, we can describe and use the artifacts that we uncover during an incident investigation. We may even obtain IOCs from third parties if the adversary or the attack is known. For example, CISA publishes the IOCs in a format called `STIX` (`Structured Threat Information eXpression`). STIX is an open-source, machine-readable language and serialization format, primarily in JSON, used to exchange cyber threat intelligence (CTI) in a standardized and consistent way.

As an example, in [this report](https://www.cisa.gov/news-events/alerts/2025/08/06/cisa-releases-malware-analysis-report-associated-microsoft-sharepoint-vulnerabilities), we can check the "Downloadable copy of IOCs associated with this malware" section for the STIX file, which contains the IOCs in JSON format.

To leverage IOCs, we will have to deploy an `IOC-obtaining/IOC-searching tool` (native or third-party and possibly at scale). A common approach is to utilize `WMI` or `PowerShell` for IOC-related operations in Windows environments.

A word of caution! During an investigation, we have to be extra careful to prevent the credentials of our highly privileged user(s) from being cached when connecting to (potentially) compromised systems (or any systems, really). More specifically, we need to ensure that only connection protocols and tools that don't cache credentials upon a successful login are utilized (such as `WinRM`). Windows logons with `logon type 3 (Network Logon)` typically don't cache credentials on the remote systems. The best example of "know your tools" that comes to mind is "PsExec". When "PsExec" is used with explicit credentials, those credentials are cached on the remote machine. When "PsExec" is used without credentials through the session of the currently logged-on user, the credentials are not cached on the remote machine. This is a great example of demonstrating how the same tool leaves different tracks, so we must be aware.

### Identification Of New Leads & Impacted Systems
After searching for IOCs, we expect to have some hits that reveal other systems with the same signs of compromise. These hits may not be directly associated with the incident we are investigating. Our IOC could be, for example, too generic. We need to identify and `eliminate false positives`.In this case, we should prioritize the ones we will focus on, ideally those that can provide us with new leads after a potential forensic analysis.

## Containment, Eradication, and Recovery Stage
When the investigation is complete and we have understood the type of incident and the impact on the business (based on all the leads gathered and the information assembled in the timeline), it is time to enter the containment stage to prevent the incident from causing more damage.

![Incident response flow titled “Containment, Eradication, and Recovery Stage.” Steps listed: Investigation is complete → Containment Strategy → Evidence gathering (note: Preserve Evidence) → Identify the attacking host → Eradication and Recovery → Bring systems back to normal operation. Side notes under containment: Forensic images, changing passwords, applying firewall rules, applying a system patch.](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/148/ir_stages.png)

### Containment
In this stage, we take action to prevent the spread of the incident. We divide the actions into `short-term containment` and `long-term containment`. It is important that containment actions are coordinated and executed across all systems simultaneously. Otherwise, we risk notifying attackers that we are after them, in which case they might change their techniques and tools in order to persist in the environment.

In short-term containment, the actions taken leave a minimal footprint on the systems on which they occur. Some of these actions can include placing a system in a separate/isolated VLAN, pulling the network cable out of the system(s), or modifying the attacker's C2 DNS name to a system under our control or to a non-existing one. The actions here contain the damage and provide time to develop a more concrete remediation strategy.

In long-term containment actions, we focus on persistent actions and changes. These can include changing user passwords, applying firewall rules, inserting a host intrusion detection system, applying a system patch, and shutting down systems. While performing these activities, we should keep the business and the relevant stakeholders updated

### Eradication
Once the incident is contained, eradication is necessary to eliminate both the root cause of the incident and what is left of it to ensure that the adversary is out of the systems and network. Some of the activities in this stage include removing the detected malware from systems, rebuilding some systems, and restoring others from backup. During the eradication stage, we may extend the previously performed containment activities by applying additional patches, that were not immediately required

### Recovery
In the recovery stage, we bring systems back to normal operation. Of course, the business needs to verify that a system is in fact working as expected and that it contains all the necessary data. When everything is verified, these systems are brought into the production environment. All restored systems will be subject to heavy logging and monitoring after an incident, as compromised systems tend to be targets again if the adversary regains access to the environment in a short period of time. Typical suspicious events to monitor for are:

- Unusual logons (e.g., user or service accounts that have never logged-in there before).
- Unusual processes.
- Changes to the registry in locations that are usually modified by malware.

The recovery stage in some large incidents may take months, as it is often approached in phases. During the early phases, the focus is on increasing overall security to prevent future incidents through quick wins and the elimination of low-hanging fruit. The later phases focus on permanent, long-term changes to keep the organization as secure as possible.

## Post-Incident Activity Stage

In this stage, our objective is to document the incident and improve our capabilities based on lessons learned from it. This stage gives us an opportunity to reflect on the threat by understanding what occurred, what we did, and how our actions and activities worked out. This information is best gathered and analyzed in a meeting with all stakeholders who were involved during the incident.
![[Pasted image 20260107222921.png]]

### Reporting
The final report is a crucial part of the entire process. A complete report will contain answers to questions such as:

- What happened and when?
- How did the team perform in dealing with the incident in regard to plans, playbooks, policies, and procedures?
- Did the business provide the necessary information and respond promptly to aid in handling the incident efficiently? What can be improved?
- What actions have been implemented to contain and eradicate the incident?
- What preventive measures should be put in place to prevent similar incidents in the future?
- What tools and resources are needed to detect and analyze similar incidents in the future?
  
# Analysis of Insight Nexus Breach