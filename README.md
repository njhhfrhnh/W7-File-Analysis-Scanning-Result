# W7-File-Analysis-Scanning-Result
## **Question 1: Analyse packet1.pcap and find the flag.**
<img width="605" height="321" alt="image" src="https://github.com/user-attachments/assets/389d6b42-a0e5-4580-b0f2-909742a23a86" />

Most ICMP packets were compared based on their length, identifier and payload content. It was observed that the majority of packets had consistent values, including a length of 98 bytes and similar sequential payload data.
These packets also followed a normal request-reply pattern.

<img width="605" height="271" alt="image" src="https://github.com/user-attachments/assets/66699152-e35d-4bd3-a3bd-35a35700a97f" />

However, packet 37 differed significantly as it had a smaller packet length, a different identifier value and did not have a corresponding request or reply reference.
Additionally, its payload contained a long alphanumeric string instead of the usual sequential pattern, making it suspicious.

It looks like the data of the file is base-64 encoded. So, I use `cyberchef` to decode it. 
<img width="1912" height="822" alt="image" src="https://github.com/user-attachments/assets/c09d7d0e-f29e-4a86-9514-afe60685a67b" />
Answer:
```bash
SUCTF2023{ai_is_cool}
```
