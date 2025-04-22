
## From Event to Alerts

The Soc team receives millions of alerts from thousands of system per day. SO they a security solution which generates `Alert` when a specific event or sequence of event occurs. This helps SOC teams to automate instead of manual log review.

**Alert Management Platforms**

|Solution|Examples|Description|
|---|---|---|
|SIEM  <br>System|Splunk ES,   <br>Elastic|SIEM have solid alert management capabilities and are a perfect choice for most SOC teams|
|EDR or  <br>NDR|MS Defender,  <br>CrowdStrike|While EDR and NDR provide their own alert dashboards, it is preferred to use SIEM or SOAR|
|SOAR  <br>System|Splunk SOAR,  <br>Cortex SOAR|Bigger SOC teams can use SOAR to aggregate and centralise alerts from multiple solutions|
|ITSM  <br>System|Jira,  <br>TheHive|Some teams may have a custom ticket management (ITSM) setup using a dedicated solution  <br>(The GIF above is taken from Trello, a simple tool that can be adapted to ITSM needs)|
##### L1 Role in Alert Triage
SOC L1 analysts are the first line of defence, and they are the ones who work with alerts the most. Depending on various factors, L1 analysts may receive zero to a hundred alerts a day, every one of which can reveal a cyberattack. Still, everyone in the SOC team is somehow involved in the alert triage:

- **SOC L1 analysts:**  Review the alerts, distinguish bad from good, and notify L2 analysts in case of a real threat
- **SOC L2 analysts:**  Receive the alerts escalated by L1 analysts and perform deeper analysis and remediation
- **SOC engineers:**  Ensure the alerts contain enough information required for efficient alert triage
- **SOC manager:**  Track speed and quality of alert triage to ensure that real attacks won't be missed.

---

### Alert Properties

![[Pasted image 20250420212901.png]]

| No    | Property          | Description                                                                                                                | Examples                                                                                                                         |
| ----- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **1** | Alert Time        | Shows alert creation time. Alert usually triggers  <br>a few minutes after the actual event                                | - Alert Time: March 21, 15:35<br>- Event Time: March 21, 15:32                                                                   |
| **2** | Alert Name        | Provides a summary of what happened,  <br>based on the detection rule's name                                               | - Unusual Login Location<br>- Email Marked as Phishing<br>- Windows RDP Bruteforce<br>- Potential Data Exfiltraton               |
| **3** | Alert Severity    | Defines the urgency of the alert,  <br>initially set by detection engineers,  <br>but can be altered by analysts if needed | - (🟢) Low / Informational<br>- (🟡) Medium / Moderate<br>- (🟠) High / Severe<br>- (🔴) Critical / Urgent                       |
| **4** | Alert Status      | Informs if somebody is working on the alert  <br>or if the triage is done                                                  | - (🆕) New / Unassigned<br>- (🔄) In Progress / Pending<br>- (✅) Closed / Resolved<br>- And often other custom statuses          |
| **5** | Alert Verdict     | Also called alert classification,  <br>explains if the alert is a real threat or noise                                     | - (🔴) True Positive / Real Threat<br>- (🟢) False Positive / No Threat<br>- And often other custom verdicts                     |
| **6** | Alert Assignee    | Shows the analyst that was assigned  <br>or assigned themselves to review the alert                                        | - Assignee can sometimes be called alert owner<br>    <br>- Assignee takes responsibility for their alerts                       |
| **7** | Alert Description | Explains what the alert is about,  <br>usually in three sections on the right                                              | - The logic of the alert generating rule<br>- Why this activity can indicate an attack<br>- Optionally, how to triage this alert |
| **8** | Alert Fields      | Provides SOC analysts' comments  <br>and values on which the alert was triggered                                           | - Affected Hostname<br>- Entered Commandline<br>- And many more, depending on the alert                                          |

---

### Alert Prioritisation

Instead of skimming through each alert, the process of deciding which alert to take upon is Alert prioritisation, a crucial step to ensure timely detection of a threat especially with many in queue.

#### Picking the Right Alert

Every SOC team decides on its own prioritisation rules and usually automates them by setting the appropriate alert sorting logic in SIEM or EDR. Below, you may see the generic, simplest, and most commonly used approach:

1. **Filter the alerts**  
    Make sure you don't take the alert that other analysts have already reviewed, or that is already being investigated by one of your teammates. You should only take new, yet unseen and unresolved alerts.
2. **Sort by severity**  
    Start with critical alerts, then high, medium, and finally low. This is because detection engineers design rules so that critical alerts are much more likely to be real, major threats and cause much more impact than medium or low ones.
3. **Sort by time**  
    Start with the oldest alerts and end with the newest ones. The idea is that if both alerts are about two breaches, the hacker from the older breach is likely already dumping your data, while the "newcomer" has just started the discovery.

---

## Alert Triage

Alert review by SOC Analyst is known as Alert Triage, Alert Handling, Alert Processing,Alert Investigation or Alert Analysis.

![[Pasted image 20250420221440.png]]

###### Initial Action
The first step is to ensure you take ownership of the alert and avoid interfering alerts being handled by other analysts, and ensure you are fully prepared to proceed with detailed investigation.

First, you assign alert to yourself then change it to `In Progress` and familiarise with alert details like its name,desc and key indicators.

###### Investigation
The most complex step requiring to apply technical knowledge and experience to understand activity and properly analyse its legitimacy in SIEM or EDR logs. To support L1 Analysts, some teams develops `WorkBooks` (Also known as Playbooks or RunBooks) comes with instructions on how to investigate a specific category of alerts. Incase of unavailability of workbooks, some key recommendations are:
 
 1. Understand who is under threat, like the affected user, hostname, cloud, network, or website
2. ote the action described in the alert, like whether it was a suspicious login, malware, or phishing
3. eview surrounding events, looking for suspicious actions shortly after or before the alert
4. se threat intelligence platforms or other available resources to verify your thoughts

###### Final Actions
The decisions taken during investigation determines whether you found or missed potential cyber attacks. Sometimes alert require actions like `Escalation` to senior levels like L2.




