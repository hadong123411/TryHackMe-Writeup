# Protocols and Servers

## Overview

This room introduced several common network protocols and explained how they operate at the application layer. The room covered Telnet, HTTP, FTP, SMTP, POP3, and IMAP, including their default ports, common commands, and security implications.

# Telnet

**Purpose**: Telnet is a protocol used for remote terminal access. It allows users to connect to a remote machine and execute commands through a command-line interface.

**Default Port**: TCP 23

**Key Points**:

- Sends all traffic in plaintext
- Usernames and passwords are not encrypted
- Mostly replaced by SSH

**Security Risk**

Anyone monitoring network traffic can capture login credentials and commands transmitted through Telnet.

# HTTP

**Purpose**: HTTP is used to transfer web pages and other web resources between clients and web servers.

**Default Port**: TCP 80

**Key Concepts**:

- Browser sends requests to a web server
- Server returns requested content
- Uses request methods such as GET

**Security Risk**

HTTP traffic is transmitted in plaintext and can be intercepted.

**HTTPS**

HTTPS encrypts HTTP traffic using TLS to protect sensitive information.

# FTP

**Purpose**: FTP is used to transfer files between systems.

**Default Port**: TCP 21 (Control Channel) and TCP 20 (Data Channel)

**Key Concepts**

- Uses separate control and data channels
- Supports file uploads and downloads
- Can allow anonymous login

**Security Risk**

FTP transmits usernames, passwords, and files without encryption.

**Modern Alternatives**

* SFTP
* FTPS

# SMTP

**Purpose**: SMTP is responsible for sending email between mail servers and email clients.

**Common Ports:**

- TCP 25
- TCP 587
- TCP 465

**Key Concepts**

- Used for email delivery
- Supports commands such as HELO, MAIL FROM, RCPT TO, and DATA

**Security Risk**

Plain SMTP traffic can be intercepted and manipulated.

**Additional Note**

SMTP design historically allowed sender address spoofing, which contributes to phishing attacks.

# POP3

**Purpose**: POP3 is used to retrieve emails from a mail server.

**Default Port**: TCP 110

**Key Concepts**:

- Downloads emails to the local device
- Traditionally removes emails from the server
- Useful for offline access

**Security Risk**

Credentials are transmitted in plaintext when encryption is not enabled.

# IMAP

**Purpose**: IMAP allows users to access and manage email while keeping messages stored on the mail server.

**Default Port**: TCP 143

**Key Concepts:**

- Synchronizes email across multiple devices
- Maintains folders and message status
- Supports server-side searching

**Security Advantage over POP3**

Emails remain synchronized across devices rather than being downloaded and removed.

**Security Risk**

Plain IMAP transmits credentials without encryption.

# Key Ports Summary

| Protocol | Default Port |
| -------- | ------------ |
| Telnet   | 23           |
| HTTP     | 80           |
| FTP      | 21           |
| SMTP     | 25           |
| POP3     | 110          |
| IMAP     | 143          |

# Security Lessons Learned

- Legacy protocols often transmit data in plaintext.
- Unencrypted protocols expose credentials to attackers.
- HTTPS, SSH, SFTP, and encrypted email protocols are preferred in modern environments.
- Understanding protocol behaviour helps during reconnaissance and penetration testing.

## Skills Learned

### Technical Skills

* Identified common network services and their default ports
* Connected to network services using Telnet
* Authenticated to FTP, POP3, and IMAP services
* Retrieved information from mail services
* Interacted with services using command-line clients
* Enumerated network protocols during reconnaissance

### Cybersecurity Concepts

* Cleartext communication
* Email infrastructure
* Client-server architecture
* Protocol enumeration
* Email authentication workflow
* Security implications of legacy protocols

### Tools Used

* TryHackMe AttackBox
* Telnet
* FTP Client
* POP3 Commands
* IMAP Commands

# Screenshots

## HTTP Connection

<img width="434" height="293" alt="image" src="https://github.com/user-attachments/assets/87954b63-a2c2-483c-ba37-d35d3b0aa688" />


## FTP Client Connection

**Description:**
Successfully connected to the FTP service and reviewed FTP authentication and file transfer functionality.

**What this demonstrates:**

- FTP communication
- Authentication process
- File transfer concepts

<img width="604" height="427" alt="image" src="https://github.com/user-attachments/assets/662ca065-a4c9-4e01-825b-e53bc64cd8fb" />

## SMTP Service Interaction

**Description:**
Connected to the SMTP service and observed how email messages are transmitted between servers.

**What this demonstrates:**

* SMTP communication
* Email delivery process
* Mail transfer concepts

<img width="581" height="80" alt="image" src="https://github.com/user-attachments/assets/40a962b9-b2b4-4e47-8c04-e221ab0b8081" />

## POP3 Authentication

**Description:**
Authenticated to the POP3 service using provided credentials and executed commands to inspect the mailbox.

**What this demonstrates:**

* Email retrieval protocols
* POP3 authentication
* Mailbox enumeration

<img width="411" height="178" alt="image" src="https://github.com/user-attachments/assets/eda8058f-448c-42ff-89c4-210163cfd696" />

# What I Learned

- Telnet allows direct interaction with network services but sends all traffic in plaintext.
- FTP uses separate control and data channels for transferring files.
- SMTP is responsible for sending emails between mail systems.
- POP3 retrieves emails from a mail server using a download-focused approach.
- IMAP synchronizes email across multiple devices while keeping messages on the server.
- Legacy protocols often expose credentials and sensitive information if encryption is not used.
- Understanding these protocols helps during reconnaissance, enumeration, traffic analysis, and incident investigations.

# Challenges

**Understanding Email Infrastructure**

The relationship between MUA, MSA, MTA, and MDA was initially difficult to understand because multiple components work together to deliver email. The diagrams and examples helped clarify how each component contributes to the overall email delivery process.

**Distinguishing POP3 and IMAP**

At first, both protocols appeared to perform the same function. After reviewing their workflows, it became clear that POP3 focuses on downloading mail, while IMAP focuses on synchronization across devices.

**Remembering Protocol Ports**

Several protocols were introduced within the same room, making it necessary to create a port summary table to remember their default ports.

# Reflection

This room provided a strong foundation in understanding how common application-layer protocols operate. While modern systems often use encrypted alternatives such as SSH, HTTPS, SFTP, and IMAPS, legacy protocols are still encountered during security assessments and internal network environments.

The practical exercises helped bridge the gap between theory and real-world usage by allowing direct interaction with services through command-line clients. This experience strengthened my understanding of how protocols communicate and highlighted the security risks associated with transmitting sensitive information in plaintext.
