
SOC refers to as Security Operations Center is a facility in enterprises where the information security team continuously monitors and analyzes the security of organization.

The main motive is to detect ,analyze and respond to cybersecurity incidents using technology ,people and process.

**Types of SOC Models**

Based upon security needs and Budget of the enterprise ,there are few types of Security Operations Center

![[Pasted image 20240708115501.png]]

**In-House SOC**

This a team formed  and managed by an organization itself that build its own cybersecurity team and plans to tackle its cybersecurity challenges itself. The Organization considering to have an internal SOC should have a budget to support its continuity.


**Virtual SOC**

This type of SOC Team doesn't have a permanent facility and works remotely from various location


**Co-Managed SOC**

The Co-Managed SOC consists of a SOC staff of an organization working with an external Managed Security Service Provider(MSSP).

Co-ordination plays crucial role in this of model.

**Command SOC**

This is type SOC model  where the SOC team oversees smaller SOC's across a large region . Organizations utilizing this model include large telecommunications providers and defense agencies.

## People, Process, and Technology

In order to establish a successfully functioning SOC environment, it requires good co-ordination between the people, process and technologies

  

Simply put, we will discuss the people, processes, and technologies required for SOC.

  

### People

A SOC teams one of the necessity is having a team of highly trained professionals that are familiar with day-to-day security alerts and attack scenarios because these attack types are constantly changing and a dynamic SOC team is required who will adapt to any new attack types and are willing to further research it.  

### Processes

To further develop your SOC structure, you need to align it with many different types of security requirements, such as NIST, PCI, and HIPAA. All processes require extreme standardization of actions to ensure nothing is left out.
  

### Technology
The SOC team must have different technologies for various tasks it covers such as Pentesting, detection, prevention and analysis and they need to follow the market and technology closely to adapt and find the best solution for the organization. Sometimes the best technology on market may not be a best product for the team due to constraints such as Budget.

## SOC Roles

### SOC Analyst

According to SOC structure the analysts roles are classified in to 3 levels. Level -1 , Level -2, Level -3. The analyst is responsible to classify the alert , look for the cause and advises on remediation.  

### Incident Responder

The incident respond officer is a first responder and performs the initial assessment of security breaches.
  

### Threat Hunter
A  Threat Hunter is professional who proactively seeks out and investigates potentials threat and vulnerabilities within an organization's network or system. The utilize a combination of manual and automates techniques to detect , isolate and mitigate Advanced persistent threats (APT's) and other sophisticated attacks that may evade traditional security measures. Threat hunters usually have a deep understanding of organization's IT infrastructure and security posture, as well as knowledge of emerging threats and attacks tactics. They quickly adapt to latest threats and  eliminate them before they can damage or disrupt the business.

### Security Engineer

Security Engineers are individuals that are primarily responsible to  maintain the security infrastructure of Security Information and Event Management(SIEM) solutions and Security Operations Center(SOC) products. For ex: A security engineer builds the connections between  SIEM and Security Orchestration, Automation and Response (SOAR) products.


### SOC Manager

A SOC manager is responsible to oversee the operations of each SOC individual and takes on other responsibilities such as budgeting , strategizing and coordinating operations.  They deal with operational issues rather than technical issues.


### SOC Analyst and Their Responsibilities

In this section, we will discuss what a SOC Analyst is, where they fit into the SOC team, and the general responsibilities of the role. It is important to review these sections carefully before learning about the technical side of the role. In this way, aspiring SOC Analyst candidates can get an idea of what their future career might look like.

  

A SOC analyst is a first perform to perform investigation on threats to a system . If a situation becomes complex he will escalate the incident to his supervisor so they can perform mitigation on the threats. The SOC analysts play a crucial role since they are first person to respond to a threat.

  

## The Advantages of Being a SOC Analyst

There are many various techniques for attack vectors and malicious software and they increase more and more every day. As an analyst you will get greater enjoyment from investigating these varying types of incidents. Even though the operating systems, security products, etc. that you use will be the same the job will feel less monotonous because you will be analyzing different incidents. Also, you may not encounter such techniques (not every week or every day).

  

## A Day in the Life of a SOC Analyst

Throughout the day, a SOC analyst typically reviews alerts in the SIEM and determines which ones are real threats. To reach a conclusion, they use various security and protection products such as Endpoint Detection and Response (EDR), Log Management, and SOAR. We will explain in detail why and how these products are used later in the training program.

  

To be a successful SOC analyst who is not dependent on security products and can correctly analyze SIEM alerts, you must have the following skills and abilities.

  

### Operating Systems

To determine what is abnormal in a system, you first need to know what is accepted as normal. For example, there are many services within the Windows operating system, and it is difficult to know which ones are suspicious without knowing which ones are or could be considered normal Windows services. Therefore, you should be familiar with how Windows/Linux operating systems work.

  

### Network

First and foremost, in this role, you will be dealing with a lot of malicious IPs and URLs, so you need to confirm that there are no devices on the network trying to connect to those addresses. Once you accomplish that, it will set the direction of the analysis.

  

This step is a bit more complicated because you may have to find a potential data leak on the network. To perform all of these functions, you need to understand the basics of networking.

  

### Malware Analysis

When dealing with most threats, you are likely to encounter some type of malicious software. To understand the real purpose of these malicious programs (they sometimes display different behaviors to fool analysts), you need to have malware analysis skills.

  

It is important to at least determine what the command and control center of the malicious file is and whether or not there is a device communicating with that address.

  

In general, we have discussed what a SOC analyst is, what the responsibilities of the role are, and what skills a SOC Analyst needs to have. As the course progresses, it will also cover technical areas, starting with SIEM.

## SIEM and Analyst Relationship

#### What is SIEM?

SIEM is a security solution that combines security information and event management, which involves real-time logging of events in an environment. The primary purpose of event logging is to detect security threats.

SIEM products have a lot of features, for this topic lets stick with SOC analysts. These analysts collect and filter data and provide alerts for suspicious events.

Example alert: If someone on a windows OS tries to enter incorrect password 20 times in less time such as 10 seconds. This is a suspicious activity and very unlikely for a person who forgotten his password to enter that many wrong passwords in such a short time. So we can use a SIEM rule/filter to detect such activity that exceeds the threshold limit.

#### Relationship between a SOC Analyst and SIEM

Although SIEM solutions have many features, SOC analysts typically only track alerts. In SIEM there are other groups/people responsible for developing configurations and rule correlations.


-> Typically, alerts are generated from data that passes through filters. 

-> These alerts are analyzed by SOC analysts 

->The SOC analysts need to determine whether generated alert isa real  threat or false threat.

![[Pasted image 20241209144256.png]]

![[Pasted image 20241209144311.png]]


**Tip** : SIEM generates quite a few false alerts and a good SOC analyst must be able to identify such alerts and provide feedback to team thereby improving the efficiency of SOC team.


## Log Management

