Enumeration (Walkthrough)

To begin the lab, I accessed the Metasploitable 2 terminal and ran the ifconfig command to examine the target machine's network configuration. This enabled me to identify its IPv4 address, 192.168.0.223, as well as its IPv6 link-local address, fe80::a00:27ff:fe94:ba85. Recording these addresses ensured that I had the correct target information for both network protocols before proceeding with further enumeration and security assessment activities.

<img width="732" height="408" alt="1ifconfig" src="https://github.com/user-attachments/assets/72dd6f7c-29cd-488f-8bf3-822a40af6913" />

Challange 1 - NetBIOS Enumeration

I used the `nbtstat -a 192.168.0.223` command in the Windows Command Prompt to perform NetBIOS enumeration on the target system. The command retrieved the NetBIOS Remote Machine Name Table, revealing the hostname **METASPLOITABLE** and confirming that the system belonged to the **WORKGROUP** workgroup. This information provided valuable insight into the target's network identity and configuration.

<img width="500" height="450" alt="Challenge 1 - nbtstat" src="https://github.com/user-attachments/assets/6ce22e85-4107-4eab-b048-e1bd34e6174e" />

Challenge 2 - Fast Nmap Scan

I executed the command `nmap -F 192.168.0.223` to perform a fast scan of the target's most commonly used ports. The scan identified several open services, including **FTP (port 21)**, **SSH (port 22)**, **Telnet (port 23)**, and **HTTP (port 80)**. These findings provided an initial overview of the services running on the target system and helped identify potential entry points for further investigation.

<img width="793" height="599" alt="Challenge 2 - nmap" src="https://github.com/user-attachments/assets/23448e95-9f08-498a-9496-30bbb3d85bb8" />

Challenge 3 - DNS Records

I conducted a comprehensive DNS enumeration of the domain **roblox.com** using three different tools. First, I used the `nslookup` command to perform a basic DNS query, which returned the domain's primary IP address. Next, I executed the `dig ANY roblox.com` command to retrieve all available DNS records in a single query, providing a broader understanding of the domain's DNS configuration. Finally, I used the `dig MX roblox.com` command to identify the Mail Exchange (MX) records, which revealed the mail servers responsible for handling and receiving email on behalf of the domain. Together, these commands provided valuable information about the domain's infrastructure and DNS setup.

<img width="1543" height="1019" alt="Challenge 3 - dig ANY" src="https://github.com/user-attachments/assets/c0b12a9e-f160-4cf9-ae40-77a09cc3806f" /> <img width="1219" height="1290" alt="Challenge 3 - dig MX" src="https://github.com/user-attachments/assets/bd84bbcc-7517-467e-a520-be12a737291d" />

<img width="1637" height="960" alt="Challenge 3 - Nslookup" src="https://github.com/user-attachments/assets/29f53cef-3251-4b3c-a903-c331ba8cfed3" />
Challenge 5 - TTL OS Fingerprinting

I performed basic network connectivity testing and initial operating system fingerprinting by executing the `ping 192.168.0.223` command from my Kali Linux terminal. The successful ICMP echo replies confirmed that the target host was reachable over the network. Additionally, I analyzed the **Time to Live (TTL)** value returned in the responses, which was **64**. This TTL value is commonly associated with **Linux/Unix-based operating systems**, providing an initial indication that the target machine was likely running a Linux environment. This information served as a useful starting point for further reconnaissance and system enumeration.

<img width="614" height="600" alt="Challenge 5 - OS Fingerprinting png" src="https://github.com/user-attachments/assets/f5d6eae9-a9ec-47bb-b302-3bfe65f52c5b" />

Challenge 9 - FTP Banner

I used the Netcat command `nc 192.168.0.223 21` to establish a connection to the target's FTP service. Upon successfully connecting, the server returned a service banner that identified the FTP software as **vsFTPd 2.3.4**. This banner-grabbing technique provided valuable information about the service version running on the target, which could be used to assess potential vulnerabilities and guide further security testing.

<img width="696" height="162" alt="Challange 9 - FTP Banner" src="https://github.com/user-attachments/assets/75ca6729-e9c4-4b6a-9c41-a6385ff7c9c6" />

Challenge 10 - Anonymous FTP Login

I attempted to access the FTP service on the target system (**192.168.0.223**) using the `ftp` command. By logging in with the username **anonymous** and leaving the password field blank, I successfully gained access to the server, confirming that anonymous authentication was enabled. After establishing the connection, I executed the `ls -al` command to view the contents of the remote directory and verified that I could interact with the file system. This finding indicates a significant security misconfiguration, as it permits unauthenticated users to access the FTP service and potentially view or download files stored on the server.

<img width="1102" height="684" alt="Challenge 10 - FTP png" src="https://github.com/user-attachments/assets/1b3bd368-9386-4b0a-9b1e-298d69daf95d" />

Challenge 11 - SMB NSE Enumeration

I began by executing the command `nmap --script smb-os-discovery -p445 192.168.0.223` to gather information about the target's operating system through SMB enumeration. The results indicated that the target was running **Samba 3.0.20** on a **Debian-based system**, providing valuable insight into the host's operating environment.

Next, I ran the command `nmap --script smb-enum-users -p445 192.168.0.223` to enumerate local user accounts configured on the system. This scan successfully revealed several accounts, including **root**, **postfix**, and **postgres**, along with their corresponding **Relative Identifiers (RIDs)**. Obtaining this information is useful for understanding the system's user structure and can assist in further security assessment and account enumeration activities.

<img width="1050" height="582" alt="Challenge 11 - SMB NSE Enumeration-OS" src="https://github.com/user-attachments/assets/cc274c2e-0763-40f6-a9bd-5a2d28274a79" />

<img width="569" height="704" alt="Challenge 11 - SMB NSE Enumeration-Users1" src="https://github.com/user-attachments/assets/a601d651-641c-45e1-9588-bbed9c62d18a" />

<img width="551" height="833" alt="Challenge 11 - SMB NSE Enumeration-Users2" src="https://github.com/user-attachments/assets/3e9cf257-02b9-4ddf-8b9e-59a8d3b24e8b" />

Challenge 16 - Version Detection

I used the command `nmap -sV 192.168.0.223` to perform service version detection on the target system. This scan identified the specific software and version numbers running on the open ports, providing a more detailed understanding of the target's services. The results revealed **vsftpd 2.3.4** running on **port 21 (FTP)**, **Apache httpd 2.2.8** on **port 80 (HTTP)**, and **Samba smbd 3.X** on the SMB service ports. Identifying precise service versions is an important step in vulnerability assessment, as it helps determine whether the services are affected by known security flaws and guides further enumeration efforts.

<img width="1075" height="671" alt="Challenge 16 - Version Detection" src="https://github.com/user-attachments/assets/88639d6b-1d1f-4e65-922c-447eac412743" />

Challenge 19 - RPC Info

I used the command `rpcinfo -p 192.168.0.223` to enumerate the Remote Procedure Call (RPC) services running on the target system. This command queried the portmapper service and returned a list of all registered RPC programs, including **NFS**, **mountd**, and **portmapper**, along with their corresponding port numbers, protocol types (TCP/UDP), and program versions. The information gathered provided valuable insight into the network services exposed by the target and helped identify additional avenues for further enumeration and security assessment.

<img width="553" height="670" alt="Challenge 19 - RPC Info" src="https://github.com/user-attachments/assets/47342f22-c688-4a31-a0eb-4ec228068e80" />

Challenge 27 - IPv6 Discovery

I used the command `nmap -6 -O fe80::a00:27ff:fe94:ba85` to perform an IPv6 network scan and operating system detection on the target host. The scan confirmed that the system was active and reachable over the IPv6 network. Additionally, the OS detection results indicated that the target was running a **Linux 2.6.X** kernel. This information verified IPv6 connectivity and provided further confirmation of the target's operating system, supporting the findings obtained during previous enumeration activities.

<img width="1310" height="652" alt="Challenge 27 - IPv6 Discovery" src="https://github.com/user-attachments/assets/d6d2e1fc-bced-4563-b0ae-7f9bc06af863" />

Challenge 29 - SMTP Enumeration via Nmap

I executed the command `nmap -p25 --script=smtp-enum-users 192.168.0.223` to enumerate valid user accounts through the SMTP service. However, the scan returned an unhandled status code when attempting the **RCPT** method, preventing successful user enumeration. Despite this limitation, the test provided insight into how the mail server responds to SMTP enumeration techniques.

Next, I ran the command `nmap -p25 --script=smtp-open-relay 192.168.0.223` to determine whether the SMTP server was configured as an open relay. The results confirmed that the server does **not** permit unauthorized email relaying, indicating that it is not vulnerable to open relay abuse. This finding suggests that the mail server has appropriate relay restrictions in place, mitigating a common SMTP security risk.

<img width="1483" height="1061" alt="Challenge 29 - SMTP Enumeration via Nmap" src="https://github.com/user-attachments/assets/dcbbf103-bdf0-4fcf-8e7e-429da9cd4071" />











