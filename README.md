# W7-File-Analysis-Scanning-Result
## **Question 1: Analyse packet1.pcap and find the flag.**

Most ICMP packets were compared based on their length, identifier and payload content. It was observed that the majority of packets had consistent values, including a length of 98 bytes and similar sequential payload data.
These packets also followed a normal request-reply pattern.
<img width="605" height="271" alt="image" src="https://github.com/user-attachments/assets/66699152-e35d-4bd3-a3bd-35a35700a97f" />


However, packet 37 differed significantly as it had a smaller packet length, a different identifier value and did not have a corresponding request or reply reference.
Additionally, its payload contained a long alphanumeric string instead of the usual sequential pattern, making it suspicious.

<img width="605" height="321" alt="image" src="https://github.com/user-attachments/assets/389d6b42-a0e5-4580-b0f2-909742a23a86" />



It looks like the data of the file is base-64 encoded. So, I use `cyberchef` to decode it. 

<img width="1912" height="822" alt="image" src="https://github.com/user-attachments/assets/c09d7d0e-f29e-4a86-9514-afe60685a67b" />
Answer:
```bash
SUCTF2023{ai_is_cool}
```

## **Question 2: Analyse packet2.pcap and find the flag.**

The packets were filtered using `tcp.len > 0` to focus on packets that contain actual payload data.

<img width="945" height="454" alt="image" src="https://github.com/user-attachments/assets/35ebd20b-88d9-4625-a30b-bfb838bc9040" />

From the filtered results, FTP traffic was identified, showing a file transfer request using the RETR command.
```bash
RETR global_thermonuclear_war.gamerules.txt
```
This indicates that a file was being transferred between the client and server.

Upon inspecting the FTP data packet, a URL was found within the payload content.
The extracted URL is:
```bash
https://tinyurl.com/yr5zprz4
```
This suggests that the actual information is hidden externally rather than directly inside the packet capture.


After opening the URL in a browser, a series of symbols were displayed.
<img width="945" height="399" alt="image" src="https://github.com/user-attachments/assets/d9312e59-273e-4bbc-a417-27453a651ce4" />

The symbols consist of boxes, lines and X/O shapes, which resemble a Tic-Tac-Toe (Club Tux / Penguin) cipher.

In CTF challenges, authors usually leave hints to guide the analysis. In this case, two hints were present:
the tic-tac-toe pattern observed in the communication and the file name itself. The word “Tux” refers to a penguin, which is commonly associated with Club Penguin’s secret code system. 
This indicates that the symbols are based on a Tic-Tac-Toe cipher.

The symbols were decoded using an online Tic-Tac-Toe cipher decoder.
<img width="945" height="535" alt="image" src="https://github.com/user-attachments/assets/2ab97db9-7586-4da0-a310-59b2db743485" />

After mapping the symbols correctly, the decoded message is:
```bash
EX MACHINA AVA
```

## **Question 3: Interpret an Nmap Output**
PORT  STATE SERVICE VERSION
21/tcp open ftp	vsftpd 2.3.4 22/tcp open ssh		OpenSSH 5.3p1 80/tcp open http			Apache 2.2.8 139/tcp open netbios-ssn
445/tcp open microsoft-ds Windows 7 Professional 7601 Service Pack 1

### Questions:

### 1. What can an attacker do with each port?

| Port | Service | Possible Attacker Actions |
|------|--------|--------------------------|
| 21 | FTP | Upload/download files, exploit misconfigurations, possible backdoor access |
| 22 | SSH | Remote login, brute-force attacks, credential access |
| 80 | HTTP | Web attacks (SQLi, XSS), directory enumeration |
| 139 | NetBIOS | Enumerate users, shares, gather system info |
| 445 | SMB | Access shared files, remote execution, lateral movement |


### 2. What vulnerabilities are likely present based on the version?

| Service | Version | Possible Vulnerabilities |
|--------|--------|-------------------------|
| vsftpd | 2.3.4 | Known backdoor vulnerability allowing remote shell access |
| OpenSSH | 5.3p1 | Outdated, vulnerable to brute-force and weak encryption |
| Apache | 2.2.8 | Outdated, vulnerable to RCE, directory traversal |
| SMB | Windows 7 SP1 | Vulnerable to EternalBlue (MS17-010) |


### 3. Which one is the highest risk and why?

The highest risk is **Port 21 - FTP (vsftpd 2.3.4)** because it contains a known backdoor vulnerability that may allow attackers to gain direct remote shell access without authentication.


### 4. What attack path can be built from this?

1. Exploit FTP backdoor → gain initial access  
2. Escalate privileges on the system  
3. Use SMB (port 445) for lateral movement  
4. Access sensitive files or systems  
5. Maintain persistence
   

### 5. What should be the remediation?

- **FTP (Port 21)**: Update vsftpd, disable anonymous access  
- **SSH (Port 22)**: Use key-based authentication, disable root login  
- **HTTP (Port 80)**: Update Apache, patch vulnerabilities  
- **SMB (Port 139/445)**: Disable if not needed, apply MS17-010 patch, restrict access  


## **Question 4: Identify the OS (OS Fingerprinting) - TTL**

### Image 1
<img width="794" height="278" alt="image" src="https://github.com/user-attachments/assets/b52c640f-fa80-495f-a3f9-1e782997c2c1" />
Answer:
```bash
TTL = 64
```
The TTL (Time To Live) value observed in the ICMP response is 64. Different operating systems use different default TTL values. Linux and Unix-based systems typically use a TTL of 64, while Windows uses 128. Therefore, based on the TTL value, the target system is identified as a Linux system.

### Image 2
<img width="440" height="301" alt="image" src="https://github.com/user-attachments/assets/b9657c75-df55-45f9-8fc1-7ec6714ad794" />
Answer:
```bash
TTL = 255
```
The observed TTL value is 255. Different systems use different default TTL values. While Linux systems typically use 64 and Windows uses 128, a TTL value of 255 is commonly associated with network devices such as routers or switches. Therefore, the target device is likely a network infrastructure device.

### Image 3
<img width="810" height="186" alt="image" src="https://github.com/user-attachments/assets/a3bfb299-7bb0-4788-9d74-98b85ab5b529" />
Answer:
```bash
TTL = 128
```
The TTL value observed in the ICMP response is 128. Different operating systems use different default TTL values. Windows systems typically use a TTL of 128, while Linux systems use 64. Therefore, the target system is identified as a Windows system.


## **Question 5: Analyse the Nessus file**

Upload to your nessus (Network_Scan.nessus) and analyse the files. Focus on critical or high findings that was identifies in analysis named “Ghostcat”.
<img width="697" height="396" alt="image" src="https://github.com/user-attachments/assets/beeb6d7c-db9c-4a07-919e-5cf4e87eace5" />


### Questions:

### 1.   What is the affected Port number?
8009

### 2.	What is the Affected protocol?
tcp (ajp13)

### 3.	What is the CVSS Score of vulnerability found?
CVSS v3.0 Base Score: 9.8

### 4.	Can you find any exploit related to this vulnerability?
Yes, exploits are available. The Ghostcat vulnerability (CVE-2020-1938) allows attackers to exploit the AJP connector to read arbitrary files from the server. In certain conditions, this can lead to remote code execution by processing malicious files as JSP.
<img width="1797" height="892" alt="image" src="https://github.com/user-attachments/assets/2afb64c2-e83d-4997-82e4-146e9a3302e4" />

### 5.	Find CVE for this vulnerability.
CVE-2020-1938
CVE-2020-1745



