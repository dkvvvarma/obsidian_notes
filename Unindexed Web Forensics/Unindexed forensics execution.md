

### Scenarios

- open browser session
- close browser  session
- system reboot/delted browser

win prefetch viewer
Osquery
regshot analysis
volatality
Rwacap & network miner


data collection considers two modes, the browser is open, and the browser is closed.

Table 2: Browsing activity

| Website                        | Activites                                                                                                                                                                | Accounts used on tor |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- |
| facebook                       | Open website  <br>Login  <br>Browsing website  <br>Download video                                                                                                        | user1@gmail.com      |
| telegram                       | Open web app (https://web.telegram.org/)  <br>Login  <br>Join group (@awlgff, @binancechinese )  <br>Chat in the group  <br>Chat with one person  <br>Delete the message | user2                |
| http://alibaba2kw6qoh6o.onion/ | Open website  <br>Login  <br>Browsing website                                                                                                                            | tor_user1            |



4 phases

Identification-acquistion

preservation - exam

analysis

presentstion


## Phase-1 :  Collection

Each department might have unique protocols for acquiring digital artefacts and evidence. However, DFIR First Responders should typically adhere to the following guidelines if there is any computer system at the scene of a crime:

- Taking an image of the RAM.
- Checking for **drive encryption**.
- Taking an image of the drive(s).

![[Pasted image 20250220091352.png]]

Process for Establishing Chain of Custody

Each department might have unique protocols regarding maintaining the chain of custody. However, DFIR First Responders should typically adhere to the following guidelines when handling digital artefacts and evidence before, during, and after collection:

- **Ensure proper documentation** of any seized materials as evidence (devices/files).
- **Hash and copy** obtained files to maintain the integrity of the original.
- Do not perform an appropriate shutdown of devices. Pull the power plug from suspect devices instead. This is to avoid data alteration as a proper shutdown may trigger anti-forensic measures.
- **Bag, Seal, and Tag the obtained artefacts** before sending them to the Forensics Laboratory.

![[Pasted image 20250122215836.png]]


![[Pasted image 20250122215900.png]]





regshot analysis

1. Install the Regshot software and save the first snapshot.
2. then, install the tor browser, use Regshot to compare the registry changes, and save the second snapshot. 
3. Third, start capture packets, use the tor browser Visit Facebook, telegram, dark web sites and save the third snapshot.
4. Fourth, close the Tor browser and close the packet capture. The browsing modes of browsing websites include regular privacy mode and customed record browsing history mode.


| S.no | Registry artefacts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Description                                                                           |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| 1    | HKLM\SYSTEM\ControlSet001\Services\bam\State\UserSettings\S-1-5-21-1501606697-969712618-2041226544-1002\\Device\HarddiskVolume3\tools\torbrowser-install-win64-10.5.5_zh-CN.exe: 63 51 4C 7C 5B AC D7 01 00 00 00 00 00 00 00 00 00 00 00 00 02 00 00 00                                                                                                                                                                                                                                                                            | Tor Browser configuration information is included in the system default configuration |
| 2    | HKLM\SYSTEM\CurrentControlSet\Services\bam\State\UserSettings\S-1-5-21-1501606697-969712618-2041226544-1002\\Device\HarddiskVolume3\tools\torbrowser-install-win64-10.5.5_zh-CN.exe: 63 51 4C 7C 5B AC D7 01 00 00 00 00 00 00 00 00 00 00 00 00 02 00 00 00                                                                                                                                                                                                                                                                        | The current system configuration contains Tor Browser configuration information       |
| 3    | HKU\S-1-5-21-1501606697-969712618-2041226544-1002\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Store\C:\tools\torbrowser-install-win64-10.5.5_zh-CN.exe: 53 41 43 50 01 00 00 00 00 00 00 00 07 00 00 00 28 00 00 00 D0 D9 70 04 E3 37 71 04 01 00 00 00 00 00 00 00 00 00 00 0A 00 21 00 00 63 1F 6E 6F 0E DE D4 01 00 00 00 00 00 00 00 00 02 00 00 00 28 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 5D 16 02 00 00 00 00 00 01 00 00 00 01 00 00 00 | Indicates where the Tor Browser is stored                                             |



Access data registry viewer

we use AccessData Registry Viewer to analyze NTUSER.dat under the user of the win10 system. Use the Tor browser as a keyword search analysis result, find relevant information under six catalog items. Through the registry analysis, the forensics personnel can obtain the tor browser version, installation location and installation time installed by the user.


| Num | Catalog item                                                                                         | description         |
| --- | ---------------------------------------------------------------------------------------------------- | ------------------- |
| 1   | NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags\1\Desktop                                           | Iconlayouts         |
| 2   | NTUSER.DAT\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Store | Path for file store |
| 3   | NTUSER.DAT\Software\Mozilla\Firefox\Launcher                                                         | Firefox Launcher    |


Bulk extractor can find a website domain name, email, and other useful information in memory, but after Tor browser close it is less than Tor browser open

![[Pasted image 20241028113707.png]]


![[Pasted image 20250220091604.png]]