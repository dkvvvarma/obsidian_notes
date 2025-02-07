
#### **Introduction**
This report outlines the methodology and technical details of how the **Havoc Command and Control (C2) Framework** was used to bypass **Windows 11 Defender**, the built-in antivirus and firewall solution in Windows 11. The goal of this exercise was to demonstrate the capabilities of advanced C2 frameworks in evading modern endpoint protection systems and to highlight potential vulnerabilities in Windows Defender's detection mechanisms.

---

### **1. Background**

#### **Windows 11 Defender**
Windows Defender, now part of Microsoft Defender, is an integrated antivirus and firewall solution designed to protect Windows systems from malware, unauthorized access, and other threats. It uses a combination of signature-based detection, heuristic analysis, and behavioral monitoring to identify and block malicious activities.

#### **Havoc C2 Framework**
Havoc is an advanced post-exploitation framework that provides red teamers and penetration testers with a powerful toolkit for executing commands, managing payloads, and maintaining persistence on compromised systems. It is known for its ability to evade detection by modern security solutions, including Windows Defender.

---

### **2. Objectives**
- Deploy and configure Havoc C2 on a Windows 11 system.
- Test the framework's ability to bypass Windows Defender's real-time protection and firewall.
- Analyze the techniques used by Havoc to evade detection.
- Document the findings and provide recommendations for improving endpoint security.

---

### **3. Methodology**

#### **Step 1: Environment Setup**
- **Target System**: Windows 11 Pro (fully updated with Windows Defender enabled).
- **Attacker System**: Kali Linux (running Havoc C2 server).
- **Network Configuration**: Both systems connected to the same local network.

#### **Step 2: Payload Generation**
- A custom payload was generated using Havoc's payload generator.
- The payload was designed to establish a reverse shell connection to the attacker's C2 server.

#### **Step 3: Payload Delivery**
- The payload was delivered to the target system using a simulated phishing attack (e.g., disguised as a legitimate executable file).
- Alternatively, the payload could be delivered via a USB drive or network share.

#### **Step 4: Execution and Evasion**
- The payload was executed on the target system.
- Havoc's built-in evasion techniques were employed to bypass Windows Defender's real-time protection and firewall.

---

### **4. Techniques Used by Havoc to Bypass Windows Defender**

#### **a. Process Injection**
- Havoc injected its malicious code into a legitimate Windows process (e.g., `explorer.exe` or `svchost.exe`).
- This technique allowed the payload to run under the guise of a trusted process, evading signature-based detection.

#### **b. Encryption and Obfuscation**
- The payload was encrypted and obfuscated to avoid detection by static analysis tools.
- Havoc used XOR encryption and custom encoding schemes to hide the payload's true nature.

#### **c. API Hooking**
- Havoc hooked critical Windows APIs to intercept and manipulate system calls.
- This allowed the framework to hide its activities from Windows Defender's behavioral monitoring.

#### **d. Firewall Bypass**
- Havoc used **port knocking** and **DNS tunneling** to establish a connection to the C2 server without triggering the Windows Firewall.
- The framework also leveraged **HTTPS** for command and control traffic, making it appear as normal web traffic.

#### **e. Persistence Mechanisms**
- Havoc established persistence on the target system by creating scheduled tasks and registry entries.
- These mechanisms ensured that the payload would execute even after a system reboot.

---

### **5. Results**

#### **a. Successful Bypass**
- Havoc successfully bypassed Windows Defender's real-time protection and firewall.
- The payload executed without triggering any alerts or being quarantined.

#### **b. Command and Control**
- A reverse shell connection was established between the target system and the Havoc C2 server.
- The attacker was able to execute commands, upload/download files, and maintain persistence on the target system.

#### **c. Detection Analysis**
- Windows Defender failed to detect the payload during initial execution and subsequent activities.
- The framework's evasion techniques effectively masked its presence from the endpoint protection system.

---

### **6. Analysis of Windows Defender's Limitations**

#### **a. Signature-Based Detection**
- Windows Defender relies heavily on signature-based detection, which is ineffective against custom or obfuscated payloads.

#### **b. Behavioral Monitoring**
- While Windows Defender includes behavioral monitoring, it was unable to detect Havoc's malicious activities due to API hooking and process injection.

#### **c. Firewall Rules**
- The Windows Firewall was unable to block Havoc's C2 traffic due to the use of HTTPS and DNS tunneling.

---

### **7. Recommendations**

#### **a. Improve Behavioral Analysis**
- Enhance Windows Defender's behavioral monitoring capabilities to detect API hooking and process injection.

#### **b. Implement Network Traffic Analysis**
- Use advanced network traffic analysis tools to identify and block suspicious C2 traffic, such as DNS tunneling.

#### **c. Regular Updates**
- Ensure that Windows Defender's signatures and detection rules are regularly updated to address emerging threats.

#### **d. User Training**
- Educate users about the risks of phishing attacks and the importance of verifying file sources before execution.

---

### **8. Conclusion**
The exercise demonstrated that advanced C2 frameworks like Havoc can effectively bypass Windows 11 Defender's protections using a combination of process injection, encryption, API hooking, and firewall evasion techniques. While Windows Defender provides a strong baseline of security, it is not infallible. Organizations must adopt a multi-layered security approach, including endpoint detection and response (EDR) solutions, network monitoring, and user training, to defend against sophisticated threats.

---

### **9. References**
- Microsoft Defender Documentation: [https://docs.microsoft.com/en-us/microsoft-365/security/defender/](https://docs.microsoft.com/en-us/microsoft-365/security/defender/)
- Havoc C2 Framework GitHub Repository: [https://github.com/HavocFramework](https://github.com/HavocFramework)
- MITRE ATT&CK Framework: [https://attack.mitre.org/](https://attack.mitre.org/)
