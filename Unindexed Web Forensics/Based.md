Titles in progress:


### Digital Forensics on the Dark Web: A Structured Approach to Artefact Recovery and Analysis

### Unveiling Anonymity: A Forensic Protocol for Dark Web Investigation

### Advanced Protocols for Extracting Forensic Evidence from Dark Web Browsing Sessions

![[D2WFP_A_Novel_Protocol_for_Forensically_Identifyin.pdf]]





Abstract

![[Pasted image 20240816092358.png]]

Ever since the Internet made into the masses and got branded as being main stream media. The Internet has been categorized as 

- Surface Web : The information that is found publicly and can be indexed by regular search                                  engines
- Deep Web :    The information that is not available to public and cannot be indexed through                               standard search engines, usually refers to the info behind paywalls.

- Dark Web :  This is a small partition of the deep web where that is well hidden and requires                             special software, configuration to access. It maintains high levels of anonymity and                       often contain sensitive info

 To tackle these circumstances, Web Forensics have made advancements in techniques , tools and methodologies. few studies have tackled these dark and deep web forensics and their

![[Pasted image 20241007103405.png]]

---

### 1. **Literature Review**

This protocol focuses on the importance of forensically investigating activities on the deep and dark web due to the increase in illegal activities carried out in these hidden online environments.

- **Dark Web and Criminal Activities**: The dark web is well-known for its criminal marketplace, including the sale of illegal goods like drugs, weapons, stolen data, and more. Criminal activities are conducted using encrypted tools such as TOR and anonymous browsers like Freenet and I2P. Forensic efforts often focus on identifying digital artefacts left behind by these activities.
  
- **Current Forensic Gaps**: Despite the advances in forensics, many existing techniques fall short when applied to the dark web, due to the encrypted, anonymous nature of the activities carried out there. Various studies stress the need for specialized tools and protocols to enhance forensic capabilities.

- **Research Progress**: Efforts in monitoring the dark web for threat intelligence have evolved with the use of machine learning, natural language processing, and data mining. These technologies help identify patterns in communication and illicit activities.

![[Pasted image 20241007103746.png]]

Overall, there is a clear gap in formalized protocols for forensic analysis of dark web activities, which this project aims to address.

---

### 2. **Requirement Analysis (SRS)**

The **Software Requirements Specification (SRS)** outlines the functionalities required to develop a system that can forensically identify, extract, and analyze dark web activities.

#### Functional Requirements:
- **Forensic Data Extraction**: The system must be able to extract artefacts from TOR browsers, including history, caches, and session data.
- **Anonymity Monitoring**: The system must be capable of tracking the use of privacy-preserving tools like TOR and Freenet.
- **Data Analysis**: The system should utilize machine learning to detect patterns in browsing activities that are potentially linked to illicit activities.
  
#### Non-Functional Requirements:
- **Scalability**: The tool must handle large datasets efficiently, especially given the volume of dark web activity.
- **Security**: Ensure that the forensic data collected is protected against tampering, and access is limited to authorized personnel.
- **Compliance**: The system should comply with legal standards for digital forensics and data privacy regulations.

---

### 3. **Tools/Methods Identification**


![[Pasted image 20241007102718.png]]

Several tools and methods have been identified for performing deep and dark web forensics based on the two documents.


- **Web Crawling**: Web crawlers can navigate dark web pages to collect hidden data, but they must remain undetected.
- **Forensic Tools**: Tools like FTK (Forensic Toolkit), Wireshark, and Magnet AXIOM are suggested for recovering artefacts from devices that access the dark web.

---

### 4. **Software Design Process**

#### a) **System Analysis**

System analysis involves identifying and understanding the various components and requirements of the system.

- **Problem Definition**: The system needs to solve the challenge of identifying artefacts left behind by users browsing the dark web through TOR and similar privacy-preserving browsers. These activities are hard to trace due to encryption and anonymity tools.
  
- **Feasibility Study**: Given the complexity of encrypted dark web activity, specialized tools like machine learning algorithms for data analysis and forensic extraction techniques for volatile data are required【10†source】【11†source】.

- **System Modeling**: Tools like DFDs (Data Flow Diagrams) can model how data flows between browsers, forensic tools, and analysis engines. The system must focus on identifying and correlating browsing history, caches, and anonymous login data【10†source】.

#### b) **System Design**

The design phase breaks down the system architecture into manageable components:

- **High-Level Design**:
  - **Architecture**: The system will include a layered approach to forensic analysis. The first layer will handle data collection from volatile memory (RAM) and persistent storage (hard disks). The second layer will focus on analyzing the artefacts using machine learning models to detect criminal activity patterns.
  
  - **Modules**: Key modules will include artefact extraction, data correlation, and report generation.
  
  - **Data Design**: Data models will be based on databases like SQLite to store the extracted artefacts for further analysis. The relational model will store timestamps, cookies, login credentials, and transaction histories【10†source】.

- **Low-Level Design**:
  - **Class Design**: Classes will represent browsing sessions, artefact recovery, and analysis processes. For example, a "Session" class will handle the metadata of each browsing activity (timestamps, URLs).
  
  - **Interface Design**: The user interface will include options for selecting the source of the forensic investigation (e.g., device type), performing scans, and generating reports【11†source】.

---

### Conclusion

The project focuses on developing a robust forensic protocol to address the challenges posed by deep and dark web activity. The literature review highlights the need for new protocols due to current gaps in forensic practices. The SRS defines both functional and non-functional requirements, ensuring the system will be scalable, secure, and compliant with regulations. Identified tools like machine learning models, NLP, and forensic toolkits are integral to the system, while the design process establishes both the high-level architecture and low-level components necessary for implementation.



| **Feature/Aspect**                      | **D2W Protocol (D2WFP)**                                                                                                                        | **Regular Dark Web Forensic Protocol**                                                             |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Scope of Investigation**              | Comprehensive protocol covering both deep and dark web forensics, including volatile memory and network forensics.                              | Focuses mainly on surface artefacts and limited dark web forensics (browser history, cache).       |
| **Artefact Coverage**                   | Identifies and extracts both volatile (RAM) and persistent (disk) artefacts, including session data, network traces, and encrypted traffic.     | Limited to basic artefacts like browsing history, cookies, and basic cached files.                 |
| **Tools Used**                          | Advanced forensic tools such as Magnet AXIOM, FTK Imager, Wireshark, and Volatility for deep memory analysis.                                   | General forensic tools like EnCase, FTK, and simpler browser-based extraction methods.             |
| **Order of Volatility (OoV)**           | Prioritizes volatile memory artefacts (RAM, process tables, ARP cache) to capture live data before shutdown.                                    | Does not emphasize the order of volatility. Often starts with disk artefact extraction.            |
| **Network Forensics**                   | Performs deep packet inspection, analyzes TOR traffic, and uses PCAP data for detailed network investigations.                                  | Basic network forensics, focusing on standard browser activity, without deep TOR traffic analysis. |
| **Data Correlation & Cross-Validation** | Correlates data from multiple sources (e.g., memory dumps, disk, network traffic) to validate findings and link artefacts.                      | Basic correlation based on disk artefacts like browser logs and cache.                             |
| **Machine Learning and NLP**            | Uses machine learning (clustering, anomaly detection) and NLP techniques for threat pattern detection from forums and chats.                    | Rarely incorporates machine learning or NLP; focuses on manual artefact review.                    |
| **Accuracy of Artefact Recovery**       | High accuracy, retrieves a wide range of artefacts (timestamps, IP addresses, login credentials, encryption keys).                              | Moderate accuracy, with limited recovery of artefacts due to lack of deep memory forensics.        |
| **Browser & Application Forensics**     | Covers a wide variety of browsers (TOR, Freenet, TAILS, I2P), including encrypted and hidden content.                                           | Focuses on common browsers and may struggle with encrypted/anonymous browsers like TOR.            |
| **Financial Activity Tracking**         | Recovers traces of cryptocurrency transactions, credit card data, and financial transactions using darknet services like CashCards and CashCow. | Limited ability to track financial artefacts from anonymous dark web markets.                      |
| **Forensic Report Generation**          | Generates comprehensive reports with detailed timelines, data correlations, and findings from various sources.                                  | Basic reports generated from disk-based artefacts and browser history.                             |
| **Anti-Forensic Techniques**            | Effectively handles anti-forensic methods (e.g., encrypted sessions, file wiping tools) used by criminals.                                      | Struggles with modern anti-forensic techniques like memory wiping and encrypted browsing.          |
| **Testing and Validation**              | Tested on various operating systems (Windows, Linux, Android, iOS) with complex forensic scenarios.                                             | Typically tested on standard operating systems with limited coverage of mobile platforms.          |


![[Pasted image 20241007110944.png]]

### Key Takeaways:
- **D2W Protocol (D2WFP)** is a more advanced and robust framework for dealing with dark and deep web forensics, utilizing advanced tools, machine learning, and memory-based techniques for artefact extraction and data analysis.
- **Regular Dark Web Forensic Protocols** tend to be more limited, focusing primarily on disk artefacts and browser history without prioritizing volatile data or complex artefact recovery from anonymous browsing tools like TOR.

This table highlights the key differences between the two approaches, demonstrating how the D2W Protocol provides deeper and more reliable forensic analysis for dark web investigations.



**System Design**

The design phase breaks down the system architecture into manageable components:

- **High-Level Design**:
  - **Architecture**: The system will include a layered approach to forensic analysis. The first layer will handle data collection from volatile memory (RAM) and persistent storage (hard disks). The second layer will focus on analyzing the artefacts using machine learning models to detect criminal activity patterns.
  
  - **Modules**: Key modules will include artefact extraction, data correlation, and report generation.
  
  - **Data Design**: Data models will be based on databases like SQLite to store the extracted artefacts for further analysis. The relational model will store timestamps, cookies, login credentials, and transaction histories.

- **Low-Level Design**:
  - **Class Design**: Classes will represent browsing sessions, artefact recovery, and analysis processes. For example, a "Session" class will handle the metadata of each browsing activity (timestamps, URLs).
  
  - **Interface Design**: The user interface will include options for selecting the source of the forensic investigation (e.g., device type), performing scans, and generating reports.