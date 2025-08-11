
## Dynamic Host Configuration Protocol (DHCP)

#### Introduction to DHCP
In a computer network, every device needs a unique IP (Internet Protocol) address to communicate with other devices. Manually assigning IP addresses to each device can be time-consuming and cause errors, especially in large networks. To resolve this issue, networks can rely on the Dynamic Host Configuration Protocol (DHCP). `DHCP` is a network management protocol used to automate the process of configuring devices on IP networks. It allows devices to automatically receive an IP address and other network configuration parameters, such as subnet mask, default gateway, and DNS servers, without manual intervention.

DHCP simplifies network management by automatically assigning IP addresses, significantly reducing the administrative workload. This automation ensures that each device connected to the network receives a unique IP address, preventing conflicts and duplication of addresses. Furthermore, DHCP recycles IP addresses that are no longer in use when devices disconnect from the network, optimizing the available address pool.

#### How DHCP Works
The DHCP process involves a series of interactions between the client (the device requesting an IP address) and the DHCP server (the service running on a network device that assigns IP addresses). This process is often referred to as `DORA`, an acronym for `Discover`, `Offer`, `Request`, and `Acknowledge`. Below we see a breakdown of DORA. Before we explore the `DORA` steps in detail, let's first clarify the roles of the `DHCP server` and the `DHCP client`:

|**Role**|**Description**|
|---|---|
|`DHCP Server`|A network device (like a router or dedicated server) that manages IP address allocation. It maintains a pool of available IP addresses and configuration parameters.|
|`DHCP Client`|Any device that connects to the network and requests network configuration parameters from the DHCP server.|

Below, we break down each step of the DORA process:

|**Step**|**Description**|
|---|---|
|`1. Discover`|When a device connects to the network, it broadcasts a **DHCP Discover** message to find available DHCP servers.|
|`2. Offer`|DHCP servers on the network receive the discover message and respond with a **DHCP Offer** message, proposing an IP address lease to the client.|
|`3. Request`|The client receives the offer and replies with a **DHCP Request** message, indicating that it accepts the offered IP address.|
|`4. Acknowledge`|The DHCP server sends a **DHCP Acknowledge** message, confirming that the client has been assigned the IP address. The client can now use the IP address to communicate on the network.|


---

# Network Address Translation(NAT)
One solution to this insufficiency issue is `Network Address Translation (NAT)`. The idea is that `NAT` allows multiple devices on a private network to share a single public IP address. This not only helps conserve the limited pool of public IP addresses but also adds a layer of security to the internal network.

#### Private vs. Public IP Addresses
`Public IP` addresses are globally unique identifiers assigned by Internet Service Providers (ISPs). Devices equipped with these IP addresses can be accessed from anywhere on the Internet, allowing them to communicate across the global network. For example, the IP address 8.8.8.8 is used for Google's DNS server, and 142.251.46.174 identifies one of Google’s web servers. These addresses ensure that devices can uniquely identify and reach each other over the internet.

`Private IP` addresses are designated for use within local networks such as homes, schools, and offices. These addresses are not routable on the global internet, meaning packets sent to these addresses are not forwarded by internet backbone routers. Defined by RFC 1918, common IPv4 private address ranges include 10.0.0.0 to 10.255.255.255, 172.16.0.0 to 172.31.255.255, and 192.168.0.0 to 192.168.255.255. This setup ensures that these private networks operate independently of the internet while facilitating internal communication and device connectivity.

#### What is NAT?

`Network Address Translation (NAT)` is a process carried out by a router or a similar device that modifies the source or destination IP address in the headers of IP packets as they pass through. This modification is used to translate the private IP addresses of devices within a local network to a single public IP address that is assigned to the router.

#### How NAT Works
The process of NAT translation begins when a device, say the laptop, sends a request to visit a website like [www.google.com](http://www.google.com/). This request packet, originating with the private IP of 192.168.1.10, is sent to the router. Here, the NAT function of the router modifies the source IP in the packet header from the private IP to the public IP of the router, 203.0.113.50.

#### Types of NAT

It's important to know that there are several types of Network Address Translation (NAT), each designed for specific networking needs. Below are the different types of NAT.

|**Type**|**Description**|
|---|---|
|`Static NAT`|Involves a one-to-one mapping, where each private IP address corresponds directly to a public IP address.|
|`Dynamic NAT`|Assigns a public IP from a pool of available addresses to a private IP as needed, based on network demand.|
|`Port Address Translation (PAT)`|Also known as NAT Overload, is the most common form of NAT in home networks. Multiple private IP addresses share a single public IP address, differentiating connections by using unique port numbers. This method is widely used in home and small office networks, allowing multiple devices to share a single public IP address for internet access.|

---

# Domain Name System(DNS)

The Domain Name System (DNS) is like the phonebook of the internet. It helps us find the right number (an IP address) for a given name (a domain such as `www.google.com`). Without DNS, we would need to memorize long, often complex IP addresses for every website we visit. DNS makes our lives easier by allowing us to use human-friendly names to access online resources.

#### Domain Names vs. IP Addresses

|**Address**|**Description**|
|---|---|
|`Domain Name`|A readable address like `www.example.com` that people can easily remember.|
|`IP Address`|A numerical label (e.g., `93.184.216.34`|

DNS bridges the gap between these two, so we can just type `www.google.com` without needing to remember the underlying IP address.

#### DNS Hierarchy
DNS is organized like a tree, starting from the root and branching out into different layers.

|**Layer**|**Description**|
|---|---|
|`Root Servers`|The top of the DNS hierarchy.|
|`Top-Level Domains (TLDs)`|Such as `.com`, `.org`, `.net`, or country codes like `.uk`, `.de`.|
|`Second-Level Domains`|For example, `example` in `example.com`.|
|`Subdomains or Hostname`|For instance, `www` in `www.example.com`, or `accounts` in `accounts.google.com`.|

![URL breakdown: Scheme, Subdomains, 2nd-Level Domain, Top-Level Domain, Page name, Root.](https://academy.hackthebox.com/storage/modules/289/DNS/DNS-2.png)

#### DNS Resolution Process (Domain Translation)
When we enter a domain name in our browser, the computer needs to find the corresponding IP address. This process is known as `DNS resolution` or `domain translation`. The steps below show how this process works.

|**Step**|**Description**|
|---|---|
|`Step 1`|We type `www.example.com` into our browser.|
|`Step 2`|Our computer checks its local DNS cache (a small storage area) to see if it already knows the IP address.|
|`Step 3`|If not found locally, it queries a `recursive DNS server`. This is often provided by our Internet Service Provider or a third-party DNS service like Google DNS.|
|`Step 4`|The recursive DNS server contacts a `root server`, which points it to the appropriate `TLD name server` (such as the `.com` domains, for instance).|
|`Step 5`|The TLD name server directs the query to the `authoritative name server` for `example.com`.|
|`Step 6`|The authoritative name server responds with the IP address for `www.example.com`.|
|`Step 7`|The recursive server returns this IP address to your computer, which can then connect to the website’s server d|

---
# Internet Architecture

`Internet Architecture` describes how data is organized, transmitted, and managed across networks. Different architectural models serve different needs—some offer a straightforward client-server setup (like a website), while others rely on a more distributed approach (like file-sharing platforms). Understanding these models helps us see why networks are designed and operated the way they are.

### Peer-to-Peer (P2P) Architecture

In a `Peer-to-Peer (P2P`) network, each node, whether it's a computer or any other device, acts as both a client and a server. This setup allows nodes to communicate directly with each other, sharing resources such as files, processing power, or bandwidth, without the need for a central server. P2P networks can be fully decentralized, with no central server involved, or partially centralized, where a central server may coordinate some tasks but does not host data.

Imagine a group of friends who want to share vacation photos with each other. Instead of uploading all the photos to a single website or server, each of them sets up a folder on their own computer that can be accessed by the others. They use a file-sharing program that connects their computers directly.

First, they install a Peer-to-Peer (P2P) file-sharing application on their computer. Then, they select the folder containing the vacation photos to share with the other friends. Everyone performs the same setup on their computers. Once everyone is connected through the P2P application, they can all browse and download photos directly from each other’s shared folders, allowing for a direct exchange of files without the need for a central server.

A popular example of Peer-to-Peer (P2P) architecture is torrenting, as seen with applications like BitTorrent. In this system, anyone who has the file, referred to as a `seeder`, can upload it, allowing others to download it from multiple sources simultaneously.

![Network diagram with interconnected devices: PC, laptop, smartphone, server, and printer.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/P2P-1.png)

In the following table, we can see the advantages and disadvantages of a Peer-to-Peer architecture.

|**Advantage**|**Description**|
|---|---|
|`Scalability`|Adding more nodes can increase total resources (storage, CPU, etc.).|
|`Resilience`|If one node goes offline, others can continue functioning.|
|`Cost distribution`|Resource burden, like bandwidth and storage, is distributed among peers, making it more cost-efficient.|

|**Disadvantage**|**Description**|
|---|---|
|`Management complexity`|Harder to control and manage updates/security policies across all nodes|
|`Potential reliability issues`|If too many peers leave, resources could be unavailable.|
|`Security challenges`|Each node is exposed to potential vulnerabilities.|

### Client-Server Architecture

The `Client-Server` model is one of the most widely used architectures on the Internet. In this setup, clients, which are user devices, send requests, such as a web browser asking for a webpage, and servers respond to these requests, like a web server hosting the webpage. This model typically involves centralized servers where data and applications reside, with multiple clients connecting to these servers to access services and resources.

Let's assume we want to check the weather forecast on a website. We start by opening the web browser on our phone or computer, and proceed to type in the website's name, e.g., `weatherexample.com`. When we press enter, the browser sends a request over the Internet to the server that hosts `weatherexample.com`. This server, a powerful computer set up specifically to store the website’s data and handle requests, receives the query and processes it by locating the requested page. It then sends back the data (regarding the weather, we requested) to our browser, which receives this information and displays the webpage, allowing us to see the latest weather updates.

![Network diagram with Internet connected to clients (PC, laptop, smartphone) and servers.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Client_Server_Arch-1.png)

A key component of this architecture is the tier model, which organizes server roles and responsibilities into layers. This enhances scalability and manageability, as well as security and performance.

#### Single-Tier Architecture

In a `single-tier` architecture, the client, server, and database all reside on the same machine. This setup is straightforward but is rarely used for large-scale applications due to significant limitations in scalability and security.

#### Two-Tier Architecture

The `two-tier` architecture splits the application environment into a client and a server. The client handles the presentation layer, and the server manages the data layer. This model is typically seen in desktop applications where the user interface is on the user's machine, and the database is on a server. Communication usually occurs directly between the client and the server, which can be a database server with query-processing capabilities.

**Note:** In a typical web application, the client (browser) does not directly interact with the database server. Instead, the browser requests web pages from a **web server**, which in turn sends its response (HTML, CSS, JavaScript) back to the browser for rendering. The web server *may* interact with an application server or database in order to formulate it's response, but in general, the scenario of a person visiting a website does not constitute a Two-Tier Architecture.

#### Three-Tier Architecture

A `three-tier` architecture introduces an additional layer between the client and the database server, known as the application server. In this model, the client manages the presentation layer, the application server handles all the business logic and processing, and the third tier is a database server. This separation provides added flexibility and scalability because each layer can be developed and maintained independently.

#### N-Tier Architecture

In more complex systems, an `N-tier` architecture is used, where `N` refers to any number of separate tiers used beyond three. This setup involves multiple levels of application servers, each responsible for different aspects of business logic, processing, or data management. N-tier architectures are highly scalable and allow for distributed deployment, making them ideal for web applications and services that demand robust, flexible solutions.

While tiered client-server architectures offer many improvements, they also introduce complexity in deployment and maintenance. Each tier needs to be correctly configured and secured, and communication between tiers must be efficient and secure to avoid performance bottlenecks and security vulnerabilities. In the following table, we can see the advantages and disadvantages of a Client-Server architecture in general.

|**Advantage**|**Description**|
|---|---|
|`Centralized control`|Easier to manage and update.|
|`Security`|Central security policies can be applied.|
|`Performance`|Dedicated servers can be optimized for their tasks.|

|**Disadvantage**|**Description**|
|---|---|
|`Single point of failure`|If the central server goes down, clients lose access.|
|`High Cost and Maintenance`|Setting up and sustaining a client-server architecture is expensive, requiring constant operation and expert management , making it costly to maintain.|
|`Network Congestion`|High traffic on the network can lead to congestion, slowing down or even disrupting connections when too many clients access the server simultaneously.|
### Hybrid Architecture

A `Hybrid` model blends elements of both `Client-Server` and `Peer-to-Peer (P2P)` architectures. In this setup, central servers are used to facilitate coordination and authentication tasks, while the actual data transfer occurs directly between peers. This combination leverages the strengths of both architectures to enhance efficiency and performance. The following example gives a high-level explanation of how a hybrid architecture works.

When we open a video conferencing app and log in, the credentials (username and password) are verified by central servers, which also manage the session by coordinating who is in the meeting and controlling access. Once we're logged in and the meeting begins, the actual video and audio data is transferred directly between our device and those of other participants, bypassing the central server to reduce lag and enhance video quality. This setup combines both models: it uses the central server for initial connection and control tasks, while the bulk of data transfer occurs in a peer-to-peer style, reducing the server load and leveraging direct, fast connections between peers. The following table refers to some of the advantages and disadvantages of a Hybrid Architecture.

![Network diagram with Internet connected to multiple devices: PC, laptop, smartphone, and server.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Hybrid_Architecture-1.png)

|**Advantage**|**Description**|
|---|---|
|`Efficiency`|Relieves workload from servers by letting peers share data.|
|`Control`|Central server can still manage user authentication, directory services, or indexing.|

|**Disadvantage**|**Description**|
|---|---|
|`Complex Implementation`|Requires more sophisticated design to handle both centralized and distributed components.|
|`Potential Single Point of Failure`|If the central coordinating server fails, peer discovery might stop.|

### Cloud Architecture

`Cloud Architecture` refers to computing infrastructure that is hosted and managed by third-party providers, such as AWS, Azure, and Google Cloud. This architecture operates on a virtualized scale following a client-server model. It provides on-demand access to resources such as servers, storage, and applications, all accessible over the Internet. In this model, users interact with these services without controlling the underlying hardware.

![Cloud network diagram with components: Servers, Apps, Database, Storage connected to Internet, linking to devices: Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Cloud_Arch-1.png)

Services like Google Drive or Dropbox are some examples of Cloud Architecture operating under the `SaaS` (Software as a Service) model, where we access applications over the internet without managing the underlying hardware. Below are five essential characteristics that define a Cloud Architecture.

|**Characteristic**|**Description**|
|---|---|
|`1. On-demand self-service`|Automatically set up and manage the services without human help.|
|`2. Broad network access`|Access services from any internet-connected device.|
|`3. Resource pooling`|Share and allocate service resources dynamically among multiple users.|
|`4. Rapid elasticity`|Quickly scale services up or down based on demand.|
|`5. Measured service`|Only pay for the resources you use, tracked with precision.|

The below table shows some of the advantages and disadvantages of the Cloud Architecture.

|**Advantage**|**Description**|
|---|---|
|`Scalability`|Easily add or remove computing resources as needed.|
|`Reduced cost & maintenance`|Hardware managed by the cloud provider.|
|`Flexibility`|Access services from anywhere with Internet connectivity.|

|**Disadvantage**|**Description**|
|---|---|
|`Vendor lock-in`|Migrating from one cloud provider to another can be complex.|
|`Security/Compliance`|Relying on a third party for data hosting can introduce concerns about data privacy.|
|`Connectivity`|Requires stable Internet access.|

### 6. Software-Defined Architecture (SDN)

`Software-Defined Networking (SDN)` is a modern networking approach that separates the control plane, which makes decisions about where traffic is sent, from the data plane, which actually forwards the traffic. Traditionally, network devices like routers and switches housed both of these planes. However, in SDN, the control plane is centralized within a software-based controller. This configuration allows network devices to simply execute instructions they receive from the controller. SDN provides a programmable network management environment, enabling administrators to dynamically adjust network policies and routing as required. This separation makes the network more flexible and improves how it's managed.

![Network diagram: Remote Servers connect to Internet, then SDN Switches, SDN Controller, and Users, linking to Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Software-Defined_Arch-1.png)

Large enterprises or cloud providers use SDN to dynamically allocate bandwidth and manage traffic flows according to real-time demands. Below is a table with the advantages and disadvantages of the Software-Defined architecture.

|**Advantage**|**Description**|
|---|---|
|`Centralized control`|Simplifies network management.|
|`Programmability & Automation`|Network configurations can be changed quickly through software instead of manually configuring each device.|
|`Scalability & Efficiency`|Can optimize traffic flows dynamically, leading to better resource utilization.|

|**Disadvantage**|**Description**|
|---|---|
|`Controller Vulnerability`|If the central controller goes down, the network might be adversely affected.|
|`Complex Implementation`|Requires new skill sets and specialized software/hardware.|

### Key Comparisons

Below is a comparison table that outlines key characteristics of different network architectures

|`Architecture`|`Centralized`|`Scalability`|`Ease of Management`|`Typical Use Cases`|
|---|---|---|---|---|
|`P2P`|Decentralized (or partial)|High (as peers grow)|Complex (no central control)|File-sharing, blockchain|
|`Client-Server`|Centralized|Moderate|Easier (server-based)|Websites, email services|
|`Hybrid`|Partially central|Higher than C-S|More complex management|Messaging apps, video conferencing|
|`Cloud`|Centralized in provider’s infra|High|Easier (outsourced)|Cloud storage, SaaS, PaaS|
|`SDN`|Centralized control plane|High (policy-driven)|Moderate (needs specialized tools)|Datacenters, large enterprises|

### Conclusion

Each architecture has its unique benefits and challenges, and in practice, we often see these models blended to balance performance, scalability, and cost. Understanding these distinctions is important for anyone planning to set up or improve network systems.

---
## Wireless Networks

A `wireless network` is a sophisticated communication system that employs radio waves or other wireless signals to connect various devices such as computers, smartphones, and IoT gadgets, enabling them to communicate and exchange data without the need for physical cables. This technology allows devices to connect to the internet, share files, and access services seamlessly over the air, offering flexibility and convenience in personal and professional environments.

|**Advantages**|**Description**|
|---|---|
|`Mobility`|Users can move around freely within the coverage area.|
|`Ease of installation`|No need for extensive cabling.|
|`Scalability`|Adding new devices is simpler than a wired network.|

|**Disadvantages**|**Description**|
|---|---|
|`Interference`|Wireless signals can be disrupted by walls, other electronics, or atmospheric conditions.|
|`Security risks`|Without proper security measures, wireless transmissions can be easier to intercept.|
|`Speed limitations`|Generally, wireless connections are slower compared to wired connections of the same generation.|

---

## Wireless Router

A `router` is a device that forwards data packets between computer networks. In a home or small office setting, a `wireless router` combines the functions of:

|**Function**|**Description**|
|---|---|
|`Routing`|Directing data to the correct destination (within your network or on the internet).|
|`Wireless Access Point`|Providing Wi-Fi coverage.|

For example, at home, our smartphones, laptops, and smart TVs all connect wirelessly to our router. The router is plugged into a modem that brings internet service from the ISP (Internet Service Provider). Below are the main components of a wireless router.

|**Component**|**Description**|
|---|---|
|`WAN (Wide Area Network) Port`|Connects to your internet source (e.g., a cable modem).|
|`LAN (Local Area Network) Ports`|For wired connections to local devices (e.g., desktop computer, printer).|
|`Antennae`|Transmit and receive wireless signals. (Some routers have internal antennae.)|
|`Processor & Memory`|Handle routing and network management tasks.|

---

## Mobile Hotspot

A `mobile hotspot` allows a smartphone (or other hotspot devices) to share its cellular data connection via Wi-Fi. Other devices (laptops, tablets, etc.) then connect to this hotspot just like they would to a regular Wi-Fi network. A mobile hotspot uses cellular data, connecting devices to the internet via a cellular network, such as 4G or 5G. The range of a hotspot is typically limited to just a few meters. Running a hotspot can also significantly drain the battery of the device creating the hotspot. For security, access to the hotspot is usually protected by a password, similar to the security measures used for a home Wi-Fi network. To better understand this concept, we can imagine that we are traveling and don’t have access to public Wi-Fi. We can activate the hotspot on our phone and connect our laptop to our phone’s Wi-Fi signal to browse the internet.

---

## Cell Tower

A `cell tower` (or `cell site`) is a structure where antennas and electronic communications equipment are placed to create a cellular network cell. This `cell` in a cellular network refers to the specific area of coverage provided by a single cell tower, which is designed to seamlessly connect with adjacent cells created by other towers. Each tower covers a certain geographic area, allowing mobile phones (and other cellular-enabled devices) to send and receive signals.

Cell towers function through a combination of radio transmitters and receivers, which are equipped with antennas to communicate over specific radio frequencies. These towers are managed by Base Station Controllers (BSC), which oversee the operation of multiple towers. BSCs handle the transfer of calls and data sessions from one tower to another when users move across different cells. Finally, these towers are connected to the core network via backhaul links, which are typically fiber optic or microwave links.

Cell towers are differentiated by their coverage capacities and categorized primarily into `macro cells` and `micro/small cells`. Macro cells consist of large towers that provide extensive coverage over several kilometers, making them ideal for rural areas where wide coverage is necessary. On the other hand, micro and small cells are smaller installations typically located in urban centers. These towers are placed in densely populated areas and fill the coverage gaps left by macro cells. To better understand the concept of a cellular network, imagine we are on a road trip, streaming music on the phone. As we move, our phone switches from one cell tower to the next to maintain connection.

---

## Frequencies in Wireless Communications

As mentioned earlier, wireless communications utilize radio waves to enable devices to connect and communicate with each other. These radio waves are emitted at specific frequencies, known as oscillation rates, which are measured in hertz (Hz). Common frequency bands for wireless networks include:

|**Frequency Bands**|
|---|
|`1.` **2.4 GHz (Gigahertz)** – Used by older Wi-Fi standards (802.11b/g/n). Better at penetrating walls, but can be more prone to interference (e.g., microwaves, Bluetooth).|
|`2.` **5 GHz** – Used by newer Wi-Fi standards (802.11a/n/ac/ax). Faster speeds, but shorter range.|
|`3.` **Cellular Bands** – For 4G (LTE) and 5G. These range from lower frequencies (700 MHz) to mid-range (2.6 GHz) and even higher frequencies for some 5G services (up to 28 GHz and beyond).|

Different frequencies play crucial roles in wireless communication due to their varying characteristics and the trade-offs between range and speed. Lower frequencies tend to travel farther but are limited in the amount of data they can carry, making them suitable for broader coverage with less data demand. In contrast, higher frequencies, while capable of carrying more data, have a much shorter range. Additionally, frequency bands can get congested as many devices operate on the same frequencies, leading to interference that degrade performance. To manage and mitigate these issues, government agencies (such as the FCC in the United States) regulate frequency allocations, ensuring orderly use of the airwaves and preventing interference among users.

---

## Summarizing

On a typical day, we might use several forms of wireless technology. At home, our wireless router provides internet access via Wi-Fi at both 2.4 GHz and 5 GHz frequencies to devices like our phone and laptop. When we leave home, our phone automatically connects to the internet using the nearest cell tower over 4G or 5G networks. While traveling abroad, we can turn on our phone’s mobile hotspot to share our cellular data with a friend’s laptop. Throughout these activities, we engage with three key wireless technologies: Wi-Fi for local wireless access, cellular networks for wide-area coverage, and a mobile hotspot for personal data sharing.

---

# Network Security
In networking, the term security refers to the measures taken to protect data, applications, devices, and systems within this network from unauthorized access or damage. The goal is to uphold and maintain the `CIA triad`:

|**Principle**|**Description**|
|---|---|
|`Confidentiality`|Only authorized users can view the data.|
|`Integrity`|The data remains accurate and unaltered.|
|`Availability`|Network resources are accessible when needed.|

In the next paragraphs, we will discuss two critical components of network security: `Firewalls` and `Intrusion Detection/Prevention Systems (IDS/IPS)`.

### Firewalls

A `Firewall` is a network security device, either hardware, software, or a combination of both, that monitors incoming and outgoing network traffic. Firewalls enforce a set of rules (known as `firewall policies` or `access control lists`) to determine whether to `allow` or `block` specific traffic. We can imagine a firewall as a security guard at the entrance of a building, checking who is allowed in or out based on a list of rules. If a visitor doesn’t meet the criteria (e.g., not on the guest list), they are denied entry.

_**The open source router/firewall [pfSense](https://www.pfsense.org/). Its large number of plugins (known as "Packages") give it a range of capabilities.**_ ![GIF showcasing the firewall rule creation in pfSense.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/pfsense.gif)

Firewalls operate by analyzing packets of data according to predefined rules and policies, commonly focusing on factors such as IP addresses, port numbers, and protocols. This process, known as traffic filtering, is defined by system administrators as permitting or denying traffic based on specific conditions, ensuring that only authorized connections are allowed. Additionally, firewalls can log traffic events and generate alerts about any suspicious activity. Below are some of the different types of firewalls.

#### 1. Packet Filtering Firewall

|**Description**|
|---|
|Operates at Layer 3 (Network) and Layer 4 (Transport) of the OSI model.|
|Examines source/destination IP, source/destination port, and protocol type.|
|`Example`: A simple router ACL that only allows HTTP (port 80) and HTTPS (port 443) while blocking other ports.|

#### 2. Stateful Inspection Firewall

|**Description**|
|---|
|Tracks the state of network connections.|
|More intelligent than packet filters because they understand the entire conversation.|
|`Example`: Only allows inbound data that matches an already established outbound request.|

#### 3. Application Layer Firewall (Proxy Firewall)

|**Description**|
|---|
|Operates up to Layer 7 (Application) of the OSI model.|
|Can inspect the actual content of traffic (e.g., HTTP requests) and block malicious requests.|
|`Example`: A web proxy that filters out malicious HTTP requests containing suspicious patterns.|

#### 4. Next-Generation Firewall (NGFW)

|**Description**|
|---|
|Combines stateful inspection with advanced features like deep packet inspection, intrusion detection/prevention, and application control.|
|`Example`: A modern firewall that can block known malicious IP addresses, inspect encrypted traffic for threats, and enforce application-specific policies.|

Firewalls stand between the internet and the internal network, examining traffic before letting it through. In a home environment, our router/modem often has a built-in firewall (software-based). In that case, it’s all in one device, and the firewall function is `inside` the router. In larger networks (e.g., business environments), the firewall is often a separate device placed after the modem/router and before the internal network, ensuring all traffic must pass through it.

![Network diagram: Internet connects to Firewall, then Router/Modem, linking to Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/Firewall-1.png)

### Intrusion Detection and Prevention Systems (IDS/IPS)

Intrusion Detection and Prevention Systems (IDS/IPS) are security solutions designed to monitor and respond to suspicious network or system activity. An Intrusion Detection System (IDS) observes traffic or system events to identify malicious behavior or policy violations, generating alerts but not blocking the suspicious traffic. In contrast, an Intrusion Prevention System (IPS) operates similarly to an IDS but takes an additional step by preventing or rejecting malicious traffic in real time. The key difference lies in their actions: an IDS detects and alerts, while an IPS detects and prevents.

_**The widely used [Suricata](https://suricata.io/) software can function as both an IDS and an IPS. Here, we see the user enable a detection rule, then begin inline monitoring.**_ ![GIF showcasing the rule enablement in Suricata.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/suricata-2.gif)

Both IDS and IPS solutions analyze network packets and compare them to known attack signatures or typical traffic patterns. This process involves:

|**Techniques**|**Description**|
|---|---|
|`Signature-based detection`|Matches traffic against a database of known exploits.|
|`Anomaly-based detection`|Detects anything unusual compared to normal activity.|

When suspicious or malicious behavior is identified, an IDS will generate an alert for further investigation, while an IPS goes one step further by blocking or rejecting the malicious traffic in real time.

_**Suricata in IDS mode.**_ ![GIF showcasing Suricata in IDS mode.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/suricata-3.gif)

Below are some of the different types of firewalls IDS/IPS.

#### 1. Network-Based IDS/IPS (NIDS/NIPS)

|**Description**|
|---|
|Hardware device or software solution placed at strategic points in the network to inspect all passing traffic.|
|`Example`: A sensor connected to the core switch that monitors traffic within a data center.|

#### 2. Host-Based IDS/IPS (HIDS/HIPS)

|**Description**|
|---|
|Runs on individual hosts or devices, monitoring inbound/outbound traffic and system logs for suspicious behavior on that specific machine.|
|`Example`: An antivirus or endpoint security agent installed on a server.|

IDS/IPS can be placed at several strategic locations in a network. One option is to position them behind the firewall, where the firewall filters obvious threats, and the IDS/IPS inspects any remaining traffic. Another common placement is in the DMZ (Demilitarized Zone), a separate network segment within the larger network directly exposed to the internet, where they monitor traffic moving in and out of publicly accessible servers. Finally, IDS/IPS solutions can also run directly on endpoint devices, such as servers or workstations, to detect suspicious activity at the host level. The following diagram shows an IDS/IPS positioned after the firewall.

![Network diagram: Internet connects to Firewall, then IPS/IDS, Router/Modem, linking to Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/IPS_IDS-1.png)

### Best Practices

Here are the best practices for enhancing network security, summarized in the following table:

|**Practice**|**Description**|
|---|---|
|`Define Clear Policies`|Consistent firewall rules based on the principle of `least privilege` (only allow what is necessary).|
|`Regular Updates`|Keep firewall, IDS/IPS signatures, and operating systems up to date to defend against the latest threats.|
|`Monitor and Log Events`|Regularly review firewall logs, IDS/IPS alerts, and system logs to identify suspicious patterns early.|
|`Layered Security`|Use `defense in depth` (a strategy that leverages multiple security measures to slow down an attack) with multiple layers: Firewalls, IDS/IPS, antivirus, and endpoint protection to cover different attack vectors.|
|`Periodic Penetration Testing`|Test the effectiveness of the security policies and devices by simulating real attacks.|

---

## Data Flow Example

Based on the knowledge we have gained from the previous sections, the following paragraphs will show precisely what happens when a user tries to access a website from their laptop. Below is a breakdown of these events in a client-server model.

#### 1. Accessing the Internet

Let's imagine a user using their laptop to connect to the internet through their home Wireless LAN (WLAN) network. As the laptop is connecting to this network, the following happens:

|**Steps**|
|---|
|The laptop first identifies the correct wireless network/SSID|
|If the network uses WPA2/WPA3, the user must provide the correct password or credentials to authenticate.|
|Finally, the connection is established, and the DHCP protocol takes over the IP configuration.|

#### 2. Checking Local Network Configuration (DHCP)

When a user opens a web browser (such as Chrome, Firefox, or Safari) and types in [www.example.com](http://www.example.com/) to access a website, the browser prepares to send out a request for the webpage. Before a packet leaves the laptop, the operating system checks for a valid IP address for the local area network.

|**Steps**|**Description**|
|---|---|
|`IP Address Assignment`|If the laptop does not already have an IP, it requests one from the home router's `DHCP` server. This IP address is only valid within the local network.|
|`DHCP Acknowledgement`|The DHCP server assigns a private IP address (for example, _192.168.1.10_) to the laptop, along with other configuration details such as subnet mask, default gateway, and DNS server.|

#### 3. DNS Resolution

Next, the laptop needs to find the IP address of `www.example.com`. For this to happen, the following steps must be taken.

|**Steps**|**Description**|
|---|---|
|`DNS Query`|The laptop sends a DNS query to the DNS server, which is typically an external DNS server provided by the ISP or a third-party service like Google DNS.|
|`DNS Response`|The DNS server looks up the domain `www.example.com` and returns its IP address (e.g., 93.184.216.34).|

#### 4. Data Encapsulation and Local Network Transmission

Now that the laptop has the destination IP address, it begins preparing the data for transmission. The following steps occur within the `OSI/TCP-IP` model:

|**Steps**|**Description**|
|---|---|
|`Application Layer`|The browser creates an HTTP (or HTTPS) request for the webpage.|
|`Transport Layer`|The request is wrapped in a TCP segment (or UDP, but for web traffic it's typically TCP). This segment includes source and destination ports (HTTP default port 80, HTTPS default port 443).|
|`Internet Layer`|The TCP segment is placed into an IP packet. The source IP is the laptop's private IP (e.g., 192.168.1.10), and the destination IP is the remote server’s IP (93.184.216.34).|
|`Link Layer`|The IP packet is finally placed into an Ethernet frame (if we're on Ethernet) or Wi-Fi frame. Here, the MAC (Media Access Control) addresses are included (source MAC is the laptop's network interface, and destination MAC is the router's interface).|

When the encapsulated frame is ready, the laptop checks its ARP table or sends an ARP request to find the MAC address of the default gateway (the router). Then, the frame is sent to the router using the router’s MAC address as the destination at the `link layer`.

#### 5. Network Address Translation (NAT)

Once the router receives the frame, it processes the IP packet. At this point, the router replaces the private IP (192.168.1.10) with its public IP address (e.g., 203.0.113.45) in the packet header. This process is known as `Network Address Translation (NAT)`. Next, the router forwards the packet to the ISP's network, and from there, it travels across the internet to the destination IP (93.184.216.34). During this process, the packet goes through many intermediate routers that look at the destination IP and determine the best path to reach that network.

#### 6. Server Receives the Request and Responds

Upon reaching the destination network, the server's firewall, if there is one, checks if the incoming traffic on port 80 (HTTP) or 443 (HTTPS) is allowed. If it passes firewall rules, it goes to the server hosting `www.example.com`. Next, the web server software (e.g., Apache, Nginx, IIS) receives and processes the request, prepares the webpage (HTML, CSS, images, etc.), and sends it back as a response.

The server's response process follows a similar path in reverse. Its IP (93.184.216.34) is now the source, and our home router's public IP (203.0.113.45) is the destination. When the packet reaches our home router (203.0.113.45), NAT ensures it is mapped back to the laptop's private IP (192.168.1.10).

#### 7. Decapsulation and Display

Finally, our laptop receives the response and strips away the Ethernet/Wi-Fi frame, the IP header, and the TCP header, until the application layer data is extracted. The laptop's browser reads the HTML/CSS/JavaScript, and ultimately displays the webpage.

#### Data Flow Diagram

Below is a flow chart showing the complete journey of a user accessing a website on the internet.

![Network process: PC connects to WLAN, sends DHCP IP request, receives response, sends DNS query via Router to DNS Server, receives response, sends HTTP request to Web Server, receives response, renders webpage.](https://academy.hackthebox.com/storage/modules/289/Data_Flow/Data_Flow-1-New.png)

