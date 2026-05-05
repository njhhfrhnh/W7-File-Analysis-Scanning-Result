# W7-File-Analysis-Scanning-Result
## **Question 1: Analyse packet1.pcap and find the flag.**

<img width="605" height="271" alt="image" src="https://github.com/user-attachments/assets/66699152-e35d-4bd3-a3bd-35a35700a97f" />

Most ICMP packets were compared based on their length, identifier and payload content. It was observed that the majority of packets had consistent values, including a length of 98 bytes and similar sequential payload data.
These packets also followed a normal request-reply pattern.

<img width="605" height="321" alt="image" src="https://github.com/user-attachments/assets/389d6b42-a0e5-4580-b0f2-909742a23a86" />


However, packet 37 differed significantly as it had a smaller packet length, a different identifier value and did not have a corresponding request or reply reference.
Additionally, its payload contained a long alphanumeric string instead of the usual sequential pattern, making it suspicious.

It looks like the data of the file is base-64 encoded. So, I use `cyberchef` to decode it. 
<img width="1912" height="822" alt="image" src="https://github.com/user-attachments/assets/c09d7d0e-f29e-4a86-9514-afe60685a67b" />
Answer:
```bash
SUCTF2023{ai_is_cool}
```

## **Question 2: Analyse packet2.pcap and find the flag.**
<img width="945" height="454" alt="image" src="https://github.com/user-attachments/assets/35ebd20b-88d9-4625-a30b-bfb838bc9040" />

The packets were filtered using `tcp.len > 0` to focus on packets that contain actual payload data.
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

<img width="945" height="399" alt="image" src="https://github.com/user-attachments/assets/d9312e59-273e-4bbc-a417-27453a651ce4" />
After opening the URL in a browser, a series of symbols were displayed.
The symbols consist of boxes, lines and X/O shapes, which resemble a Tic-Tac-Toe (Club Tux / Penguin) cipher.

In CTF challenges, authors usually leave hints to guide the analysis. In this case, two hints were present:
the tic-tac-toe pattern observed in the communication and the file name itself. The word “Tux” refers to a penguin, which is commonly associated with Club Penguin’s secret code system. 
This indicates that the symbols are based on a Tic-Tac-Toe cipher.

<img width="945" height="535" alt="image" src="https://github.com/user-attachments/assets/2ab97db9-7586-4da0-a310-59b2db743485" />

The symbols were decoded using an online Tic-Tac-Toe cipher decoder.
After mapping the symbols correctly, the decoded message is:
```bash
EX MACHINA AVA
```


