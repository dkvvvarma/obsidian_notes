 
### Abstract

This report outlines the activities and findings from a one-month internship that focused on understanding IT infrastructure, conducting vulnerability assessments, and performing log analysis. The first two weeks were dedicated to acquiring a thorough understanding of IT networks, followed by a week of studying advanced cybersecurity models, including post-exploitation and auditing practices. The final week was spent actively auditing the network infrastructure by conducting host discovery and vulnerability scans using Nessus and performing log analysis using Splunk. The goal of this internship was to assess the security posture of the organization's IT systems and identify potential vulnerabilities and threats, with an emphasis on strengthening the organization's cybersecurity defenses.

### Introduction

The internship was designed to provide practical experience in IT security, specifically in auditing and vulnerability management. The program offered exposure to the core elements of network security, including the importance of maintaining and enhancing security measures through continuous auditing and monitoring. During the internship, I conducted vulnerability scans using Nessus to identify security gaps and analyzed system logs with Splunk to detect anomalies and potential threats. This hands-on experience was invaluable in understanding the intricacies of IT auditing, post-exploitation processes, and network security management. The primary objective was to identify security weaknesses, monitor system behavior, and implement timely interventions to mitigate risks.

### Literature Survey

The significance of vulnerability management and log analysis is well-documented in cybersecurity literature. Various studies emphasize that regular vulnerability scanning is essential for discovering and addressing security gaps in organizational infrastructure. Nessus is a widely used tool recognized for its comprehensive vulnerability scanning capabilities across various operating systems and network devices. Meanwhile, Splunk has established itself as a leading platform for log management and analysis, offering insights into system behavior and detecting anomalous activity. Together, these tools form a critical part of an organization’s security framework, providing both preventative and detective controls to mitigate risks and respond to potential security incidents in real time.

### Existing System

The organization’s existing security system lacked regular, systematic vulnerability assessments, which posed a significant risk of unpatched vulnerabilities being exploited. Furthermore, the absence of a consistent and comprehensive log analysis process meant that some security incidents could go unnoticed, thereby increasing the organization’s exposure to threats. This gap highlighted the need for an improved, structured approach to monitoring and protecting the IT infrastructure from potential attacks.

### Problem Statement

The organization faced a challenge in effectively identifying and mitigating security vulnerabilities within its IT infrastructure. The absence of routine vulnerability assessments and limited log analysis capabilities increased the risk of undetected security threats and potential data breaches. Therefore, a systematic approach was required to improve the security monitoring processes and ensure timely identification and resolution of vulnerabilities and security incidents.

### Proposed System

To address these security gaps, the proposed system involves implementing regular Nessus vulnerability scans to ensure comprehensive coverage of the IT environment and identify security weaknesses. Additionally, Splunk will be used for thorough log analysis to detect and respond to potential security incidents. The combination of these tools will provide continuous monitoring of the network, facilitate timely detection of vulnerabilities, and enhance the organization's overall cybersecurity defenses. This integrated approach is designed to reduce security risks and ensure that vulnerabilities and threats are detected and addressed promptly.

### Modeling


The model for improving security and auditing includes the following phases:
1. **Learning Phase**: The first phase involves gaining a deep understanding of the organization's IT infrastructure and security principles, including network topology, firewall configurations, and system architecture.
2. **Vulnerability Scanning**: Using Nessus, periodic vulnerability scans will be conducted on the organization's IT assets. These scans will help identify weaknesses such as outdated software, misconfigurations, and potential entry points for attackers.
3. **Log Analysis**: Splunk will be used to collect and analyze system logs across various servers, network devices, and applications. This analysis will help identify unusual patterns, such as repeated login attempts, unauthorized access, and other indicators of compromise.

### Software Used & Hardware and Software Requirements
**Software Used**:
- **Nessus**: Used for performing comprehensive vulnerability assessments and identifying security flaws within the network.
- **Splunk**: Employed for log management and analysis, enabling real-time monitoring of system activities and identifying potential security incidents.

**Hardware and Software Requirements**:
- **Hardware**: High-performance workstations with sufficient processing power, memory, and storage capacity to support Nessus and Splunk operations, especially during large-scale scans and log analysis.
- **Software**: Access to licensed versions of Nessus and Splunk to ensure the full functionality of the tools. Additionally, secure access to the organization's IT infrastructure is necessary to perform scans and collect log data.

### Testing & Result Analysis
**Testing**:
- Conducted Nessus vulnerability scans on various network components, including servers, routers, and switches, to identify potential security issues such as unpatched vulnerabilities, weak configurations, and outdated software.
- Performed log analysis using Splunk on system logs collected from different network segments, with a focus on identifying security anomalies, unauthorized access attempts, and other suspicious activities.

**Result Analysis**:
- The Nessus scans revealed several vulnerabilities, which were classified based on their severity levels—ranging from low-risk issues, such as minor misconfigurations, to critical vulnerabilities that required immediate remediation.
- The Splunk analysis provided valuable insights into the system's operational behavior and identified multiple security events, including suspicious login attempts, access to restricted files, and unusual network traffic patterns. These incidents were flagged for further investigation and response.

### Conclusion & Future Work


The internship effectively demonstrated the value of regular vulnerability assessments and log analysis in strengthening the security posture of an organization. The use of Nessus and Splunk proved to be instrumental in identifying and mitigating security risks. Moving forward, it is recommended to refine the processes for vulnerability scanning and log analysis, possibly integrating additional security tools such as intrusion detection systems (IDS) and endpoint detection and response (EDR) platforms. Future efforts should also focus on training IT staff to maintain and enhance the organization's security measures, ensuring that they remain vigilant and proactive in the face of evolving cyber threats.

### References
1. Tenable, "Nessus: Vulnerability Assessment Tool." [Online](https://www.tenable.com/products/nessus)
2. Splunk, "Splunk: The Data-to-Everything Platform." [Online](https://www.splunk.com)
3. Various academic journals and articles on IT security, vulnerability management, and log analysis.
