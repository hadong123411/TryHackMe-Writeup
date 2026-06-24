# TryHackMe Write-up: Protocols and Servers 2

## Overview

This room explored common network protocols, traffic interception techniques, encrypted communications, secure file transfers, and password attacks. The room demonstrated how attackers can capture sensitive information from network traffic, perform Man-in-the-Middle (MITM) attacks, and exploit weak authentication mechanisms. It also introduced modern defensive technologies such as TLS, certificates, and secure authentication methods.

## Objectives

* Understand packet sniffing and network traffic analysis.
* Learn how credentials can be captured from insecure protocols.
* Explore common Man-in-the-Middle attack techniques.
* Understand how TLS protects web communications.
* Learn secure remote access and file transfer protocols.
* Perform password attacks using Hydra.
* Understand modern authentication and defensive measures.

# Key Concepts Learned

## Sniffing Attacks

Packet sniffing is the process of capturing and analyzing network traffic traveling across a network. Attackers can use sniffing techniques to intercept sensitive information such as usernames, passwords, cookies, and session tokens.

Sniffing is especially dangerous when protocols transmit data in plaintext.

## Packet Capture Tools

Several packet analysis tools were introduced:

- **tcpdump**: A command-line packet capture utility commonly used on Linux systems.

  Example:

  ```bash
  tcpdump -i eth0
  ```
- **Wireshark**: A graphical packet analysis tool that allows detailed inspection of network traffic and protocol communications.

- **TShark**: The command-line version of Wireshark.

  Example:
  
  ```bash
  tshark -i eth0
  ```

These tools are commonly used by security analysts for network monitoring, troubleshooting, and incident investigations.


## Capturing POP3 Credentials

POP3 transmits authentication credentials in plaintext unless additional encryption mechanisms are used.

By capturing network traffic with tools such as Wireshark, attackers can view usernames and passwords directly from packet contents.

This demonstrates why legacy protocols without encryption are considered insecure.


## Mitigation Against Packet Sniffing

Common defensive measures include:

* Using encrypted protocols
* Implementing TLS/SSL
* Using VPNs
* Segmenting networks
* Avoiding insecure legacy services

## Man-in-the-Middle (MITM) Attacks

A MITM attack occurs when an attacker positions themselves between two communicating devices and intercepts or modifies traffic.

The victim believes they are communicating directly with the intended destination while the attacker secretly observes or alters communications.

## Common MITM Techniques

- **ARP Spoofing**: An attacker sends forged ARP messages to associate their MAC address with another device's IP address. This causes traffic to be redirected through the attacker's system.

- **DNS Spoofing**: Attackers manipulate DNS responses to redirect victims to malicious websites.

- **Rogue Access Points**: Attackers create fake wireless networks that victims connect to unknowingly. All traffic passing through the rogue access point can be monitored or modified.

**BGP Hijacking**: Attackers manipulate internet routing information to redirect large amounts of traffic through unauthorized networks.This technique can affect entire organizations or regions.

## MITM Tools

Several offensive security tools were introduced:

- **Bettercap**: Framework for network attacks, traffic interception, and packet manipulation.

- **Ettercap**: A popular tool for ARP spoofing and network interception.

- **mitmproxy**: An interactive proxy used to inspect and modify web traffic.

- **Responder**: A tool used to capture authentication credentials on Windows networks.
- 
## MITM Against Encrypted Traffic

Encryption makes MITM attacks significantly more difficult.

**Attackers may attempt to**:

* Present fraudulent certificates
* Downgrade connections
* Exploit user trust decisions
* Abuse compromised certificate authorities

Modern browsers include numerous protections against these techniques.

## Modern Defenses Against MITM

Common protections include:

* HTTPS
* TLS
* Certificate validation
* HSTS
* VPNs
* Multi-Factor Authentication
* Secure DNS technologies

## SSL and TLS

**SSL (Secure Sockets Layer)**: is the predecessor to TLS. Disadvantages of SSL:
  - Performance Overhead. Encryption and decryption of outgoing and incoming data requires CPU resources
  - Increased complexity
  - Certificate costs and administration
  - SSL doesn't protect everything 

**TLS (Transport Layer Security)**: is the modern standard used to protect communications across networks.

TLS provides:

* Confidentiality
* Integrity
* Authentication

## STARTTLS

STARTTLS allows an existing plaintext connection to be upgraded to an encrypted TLS connection.

It is commonly used in email protocols such as:

* SMTP
* IMAP
* POP3

## How HTTPS Works

HTTPS combines HTTP with TLS encryption.

When a user visits a secure website:

1. The browser requests a connection.
2. The server presents a certificate.
3. TLS establishes an encrypted session.
4. Data is transmitted securely.

This prevents attackers from easily reading or modifying communications.

## The TLS Handshake

The TLS handshake establishes trust between a client and server.

Key steps include:

1. Client Hello
2. Server Hello
3. Certificate exchange
4. Key agreement
5. Session key generation
6. Encrypted communication begins

This process ensures confidentiality and authenticity.

## Certificates and Trust

Certificates verify the identity of websites and services.

A certificate contains:

* Domain information
* Public key
* Issuing Certificate Authority (CA)
* Expiration details

Browsers trust certificates issued by trusted Certificate Authorities.


## Modern Certificate Ecosystem

**Modern web security relies on**:

* Certificate Authorities
* Public Key Infrastructure (PKI)
* Certificate transparency logs
* Automated certificate management

These systems help maintain trust across the internet.

## Testing TLS Configurations

**Security professionals assess TLS implementations to identify**:

* Weak ciphers
* Deprecated protocols
* Misconfigurations
* Expired certificates

Proper TLS configuration is essential for secure communications.

## SSH

**SSH (Secure Shell)**: provides encrypted remote administration of systems.

**Common uses include**:

* Remote server management
* Secure command execution
* Secure file transfers

**Example**:

```bash
ssh user@target-ip
```

---

## SSH vs FTPS vs SFTP

**SSH**

Secure remote command execution.

**FTPS**

FTP combined with TLS encryption.

**SFTP**

File transfer protocol that operates through SSH.

SFTP is generally preferred because it uses a single secure protocol and is easier to manage.

## SCP

**SCP (Secure Copy Protocol)**: transfers files securely using SSH.

**Example**:

```bash
scp file.txt user@target:/home/user/
```

SCP provides a simple method of securely transferring files between systems.


## Password Attacks

Password attacks attempt to gain unauthorized access by guessing or discovering valid credentials.

Common targets include:

* SSH services
* Web logins
* FTP services
* Databases

## Authentication Factors

Authentication can be based on:

**Something You Know**

* Passwords
* PINs

**Something You Have**

* Security tokens
* Smartphones

**Something You Are**

* Fingerprints
* Facial recognition

Using multiple factors significantly improves security.

---

## Types of Password Attacks

- **Brute Force**: Attempts every possible password combination.

- **Dictionary Attack**: Uses a list of likely passwords.

- **Credential Stuffing**: Uses credentials leaked from previous breaches.

- **Password Spraying**: Attempts a few common passwords across many accounts.

## Wordlists and Breach Data

Wordlists are collections of passwords used during password attacks.

Common sources include:

* RockYou
* Public breach databases
* Custom generated lists

Attackers frequently leverage real-world leaked passwords because many users reuse credentials.

## Hydra

**Hydra**: is a password-cracking tool used to perform online authentication attacks against various services.

**Example**:

```bash
hydra -l user -P wordlist.txt ssh://target-ip
```

**Hydra supports numerous protocols including**:

* SSH
* FTP
* HTTP
* SMB
* Telnet

## Useful Hydra Options

| Option | Description                       |
| ------ | --------------------------------- |
| -l     | Single username                   |
| -L     | Username list                     |
| -p     | Single password                   |
| -P     | Password list                     |
| -t     | Number of threads                 |
| -V     | Verbose output                    |
| -f     | Stop after first successful login |

These options allow security professionals to customize password auditing activities.

## Skills Learned

* Packet analysis using Wireshark and tcpdump
* Identifying plaintext credential exposure
* Understanding Man-in-the-Middle attacks
* Recognising network spoofing techniques
* Understanding TLS and HTTPS security
* Using SSH and SCP securely
* Understanding password attack methodologies
* Performing password auditing with Hydra
* Understanding authentication mechanisms
* Evaluating protocol security risks

# Screenshot Section

## Capturing POP3 Credentials

**Add Screenshot Here**

What it shows:

* Packet capture containing POP3 traffic.
* Plaintext username and password visible within captured packets.
* Demonstrates why unencrypted protocols are insecure.

---

## Screenshot 2 – SSH and SCP Usage

<img width="610" height="382" alt="image" src="https://github.com/user-attachments/assets/064515e1-992f-4f41-842f-06ce3080b84b" />

<img width="615" height="193" alt="image" src="https://github.com/user-attachments/assets/a8d3515f-fd9d-4b5b-8cf4-cd325d059cf7" />


What it shows:

* Successful SSH connection to the target system.
* Secure remote access using encrypted communications.
* SCP used to transfer files securely between systems.


## Screenshot 3 – Hydra Password Attack

<img width="676" height="241" alt="image" src="https://github.com/user-attachments/assets/fae176d4-9ef6-47b0-be98-98fbadd7fd4f" />

What it shows:

* Hydra executing against the SSH service.
* Successful credential discovery.
* Demonstrates the effectiveness of weak passwords and password reuse.

---

# Challenges Encountered

The most challenging part of this room was using Hydra to discover valid SSH credentials and locating the required flag afterward.

After successfully obtaining access credentials, I initially assumed the flag would be immediately visible on the system. However, I spent time searching manually before realising that the required file was actually contained within `book.txt`, which had already been downloaded.

Using:

```bash
cat book.txt
```

revealed the flag and completed the challenge. This reinforced the importance of carefully reviewing available files and understanding what has already been downloaded before spending additional time searching elsewhere.


# What I Learned

This room demonstrated how insecure protocols can expose credentials through simple packet captures and why encryption is essential for modern communications. I learned how Man-in-the-Middle attacks work, how TLS establishes trust and protects data, and how protocols such as SSH and SFTP provide secure alternatives to legacy services.

I also gained practical experience performing password attacks with Hydra and learned how attackers leverage wordlists and breached credential databases to compromise accounts. Most importantly, the room highlighted the importance of encryption, strong authentication, and proper network security controls in defending against credential theft and traffic interception.


# Reflection

This room connected several networking and security concepts together in a practical way. Rather than viewing protocols as isolated technologies, I gained a better understanding of how attackers interact with network traffic and how modern security controls are designed to prevent those attacks.

The packet capture exercises were particularly useful because they demonstrated how easily plaintext credentials can be exposed. Learning about TLS, certificates, SSH, and secure file transfer methods helped reinforce why encrypted communications are now considered a fundamental security requirement.

Overall, this room strengthened my understanding of network security, protocol analysis, authentication mechanisms, and defensive technologies that are highly relevant to both penetration testing and SOC analyst roles.
