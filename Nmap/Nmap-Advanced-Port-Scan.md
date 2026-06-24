# TryHackMe Write-up: Nmap Advanced Port Scans

## Overview

This room explored advanced Nmap scanning techniques designed to gather information while avoiding traditional detection methods. Rather than relying solely on standard TCP SYN scans, these techniques manipulate TCP flags, packet structures, and source information to identify open ports, map firewall rules, and perform stealthier reconnaissance.

The room introduced several advanced scanning methods including NULL, FIN, Xmas, Maimon, ACK, Window, Idle, and custom scans. It also covered packet fragmentation, decoy scanning, spoofing techniques, and methods for obtaining more detailed scan results.

---

## Objectives

* Understand advanced TCP scanning techniques.
* Learn how different TCP flag combinations influence target responses.
* Explore methods for firewall and filtering detection.
* Understand stealth and evasion techniques.
* Learn how Nmap gathers additional information from scan results.
* Develop a deeper understanding of TCP/IP behaviour during reconnaissance.

---

# Key Concepts Learned

## Why Advanced Scans Exist

Traditional port scans are often easy to detect because they generate predictable traffic patterns.

Advanced scans aim to:

* Bypass basic firewall rules.
* Reduce detection by intrusion detection systems.
* Identify filtering devices.
* Gather information from systems that block traditional scans.
* Improve reconnaissance accuracy.

These techniques exploit how TCP implementations handle unexpected packet combinations.

---

# TCP Flag Fundamentals

TCP communication relies on several control flags:

| Flag | Purpose                          |
| ---- | -------------------------------- |
| SYN  | Initiates a connection           |
| ACK  | Acknowledges received data       |
| FIN  | Gracefully closes a connection   |
| RST  | Resets a connection              |
| PSH  | Pushes buffered data immediately |
| URG  | Indicates urgent data            |

Many advanced scans manipulate these flags to trigger different responses from target systems.

---

## NULL Scan

A NULL scan sends a TCP packet with **no flags set**.

Example:

```bash id="5ihnzd"
nmap -sN target
```

### How It Works

According to RFC standards:

* Closed ports respond with RST.
* Open ports typically ignore the packet.

### Advantages

* Can bypass simple packet filters.
* Generates unusual traffic patterns.
* Useful against certain Unix-based systems.

### Disadvantages

* Less effective against Windows systems.
* Modern firewalls often detect or block it.

---

## FIN Scan

A FIN scan sends packets with only the FIN flag enabled.

Example:

```bash id="d7qmfv"
nmap -sF target
```

### How It Works

Open ports generally ignore FIN packets.

Closed ports return RST responses.

### Advantages

* Stealthier than traditional scans.
* May evade simple filtering rules.

### Disadvantages

* Not reliable on all operating systems.
* Modern security controls often recognize it.

---

## Xmas Scan

A Xmas scan sends packets with:

* FIN
* PSH
* URG

enabled simultaneously.

Example:

```bash id="8pqhz6"
nmap -sX target
```

The packet appears "lit up" with multiple flags, similar to a Christmas tree.

### Advantages

* Can reveal firewall behaviour.
* May bypass basic filtering mechanisms.

### Disadvantages

* Easily identifiable by modern security systems.
* Limited effectiveness against Windows hosts.

---

## Comparing NULL, FIN and Xmas Scans

These scans rely on the same principle:

| Port State | Expected Response |
| ---------- | ----------------- |
| Open       | No response       |
| Closed     | RST response      |

This behaviour allows Nmap to infer port status without initiating a full connection.

---

## TCP Maimon Scan

The Maimon scan sends FIN/ACK packets.

Example:

```bash id="4f67s5"
nmap -sM target
```

### Purpose

This technique attempts to exploit implementation differences in TCP stacks.

### Advantages

* Can occasionally bypass filtering.
* Useful against certain systems.

### Disadvantages

* Rarely more effective than FIN scans today.
* Modern operating systems have largely mitigated the behaviour.

---

## TCP ACK Scan

ACK scans are designed to map firewall rules rather than identify open ports.

Example:

```bash id="jgq3kh"
nmap -sA target
```

### How It Works

The target's response indicates whether traffic is being filtered.

Possible results:

* Filtered
* Unfiltered

### Advantages

* Excellent for firewall reconnaissance.
* Helps identify packet filtering devices.
* Provides network architecture insights.

### Disadvantages

* Cannot directly determine open ports.
* Requires interpretation alongside other scan results.

---

## Window Scan

Window scans build upon ACK scans.

Example:

```bash id="nnp5t1"
nmap -sW target
```

### Purpose

Some operating systems return different TCP window sizes depending on port status.

Nmap can use this information to estimate whether ports are open or closed.

### Advantages

* Can provide additional information when ACK scans are inconclusive.
* Useful against certain TCP implementations.

### Disadvantages

* Operating-system dependent.
* Less reliable on modern systems.

---

## Custom TCP Scans

Nmap allows users to define custom TCP flag combinations.

Example:

```bash id="6j50nk"
nmap --scanflags SYNFIN target
```

### Advantages

* Highly flexible.
* Useful for research and specialized testing.
* Can reveal unusual network behaviours.

### Disadvantages

* Requires deeper TCP knowledge.
* Results may vary significantly between systems.

---

# Spoofing and Decoys

## Source Address Spoofing

Attackers may forge source IP addresses to disguise scan origins.

### Purpose

* Obscure attribution.
* Complicate investigations.
* Evade basic logging mechanisms.

### Limitation

Responses are sent to the spoofed address, making interaction difficult.

---

## Decoy Scanning

Example:

```bash id="xfw70v"
nmap -D decoy1,decoy2,ME target
```

Nmap generates traffic from multiple apparent sources.

### Advantages

* Makes attribution more difficult.
* Increases analyst workload.
* Provides an additional layer of obfuscation.

### Disadvantages

* Does not guarantee anonymity.
* Advanced monitoring can still identify the real source.

---

# Packet Fragmentation

Example:

```bash id="g8t64m"
nmap -f target
```

### Purpose

Packet fragmentation splits scan packets into smaller fragments.

Historically, some firewalls failed to properly inspect fragmented packets.

### Advantages

* May bypass poorly configured filtering devices.
* Demonstrates how packet inspection can be evaded.

### Disadvantages

* Modern security devices typically reassemble fragments.
* Less effective today than historically.

---

# Idle (Zombie) Scanning

One of the most fascinating Nmap techniques.

Example:

```bash id="mbwg5o"
nmap -sI zombie-ip target
```

### How It Works

A third-party system known as a zombie is used to perform the scan indirectly.

The target believes the zombie initiated the scan rather than the attacker.

### Advantages

* Extremely stealthy.
* Hides the attacker's IP address.
* Reveals trust relationships and filtering behaviour.

### Disadvantages

* Requires a suitable zombie host.
* More complex than standard scans.
* Less practical in modern environments.

---

# Understanding Scan Results

## Using --reason

Example:

```bash id="8j67ut"
nmap --reason target
```

### Purpose

Displays the specific reason Nmap assigned a port state.

Example output:

```text id="ckf6lv"
80/tcp open syn-ack
22/tcp filtered no-response
```

### Advantages

* Improves result interpretation.
* Helps troubleshoot scanning issues.
* Provides greater visibility into target behaviour.

---

# Skills Learned

* Understanding advanced TCP flag manipulation.
* Performing NULL, FIN, Xmas, and Maimon scans.
* Mapping firewall behaviour with ACK scans.
* Using Window scans for additional intelligence.
* Creating custom TCP scan configurations.
* Understanding spoofing and decoy techniques.
* Performing fragmented packet scans.
* Understanding Idle (Zombie) scanning concepts.
* Interpreting detailed Nmap output.
* Analyzing TCP/IP behaviour during reconnaissance.

---

# Screenshot Section

## FIN Scan (-sF)

<img width="605" height="283" alt="image" src="https://github.com/user-attachments/assets/c5e7fc46-999e-4421-b0b1-0ed7e511de8c" />

What it shows:

- Execution of a TCP FIN scan using Nmap.
- Ports such as SSH (22), SMTP (25), HTTP (80), POP3 (110), IMAP (143), and UPnP (5000) reported as `open|filtered`.
- Demonstrates how FIN scans rely on RFC-compliant TCP behaviour where open ports typically do not respond while closed ports return RST packets.

Key Observation:

- Nmap cannot always distinguish between open and filtered ports, which is why the state is reported as `open|filtered`.

---

## Xmas Scan (-sX)

<img width="614" height="287" alt="image" src="https://github.com/user-attachments/assets/3c12eb1e-2efb-4260-b5fd-0704c300e35c" />


What it shows:

- Execution of an Xmas scan using FIN, PSH, and URG TCP flags.
- Similar results to the FIN scan, with several ports reported as `open|filtered`.
- Demonstrates how manipulating TCP flags can provide information about target systems without performing a traditional connection.

Key Observation:

- Xmas scans generate unusual traffic patterns that may bypass simple filtering devices but can also be detected by modern IDS/IPS solutions.

---

## Maimon Scan (-sM)

<img width="611" height="157" alt="image" src="https://github.com/user-attachments/assets/e455546c-d529-4c5a-a20e-58f2f51ad948" />

What it shows:

- Execution of a TCP Maimon scan.
- Nmap reports all scanned ports as closed.
- Demonstrates how the Maimon technique relies on TCP implementation differences rather than standard connection behaviour.

Key Observation:

- Modern operating systems often mitigate the behaviour that made Maimon scans useful, reducing their effectiveness in contemporary environments.

---

## ACK Scan (-sA)

<img width="608" height="236" alt="image" src="https://github.com/user-attachments/assets/6e5afaea-8c70-4342-adcd-be9409c5b2f1" />

What it shows:

- ACK scan results showing ports reported as `unfiltered`.
- Ports such as SSH, SMTP, HTTP, HTTPS, and UPnP respond to ACK probes.
- Demonstrates how ACK scans are used primarily for firewall analysis rather than port discovery.

Key Observation:

- The results indicate that packets are reaching the target and are not being blocked by a firewall, but they do not directly confirm whether the ports are open or closed.

---

## Screenshot 5 – Using --reason for Detailed Results


<img width="613" height="240" alt="image" src="https://github.com/user-attachments/assets/ce0c7a7c-380a-457c-a4b7-4cfebf38c7a4" />

What it shows:

- Nmap scan executed with the `--reason` option.
- Additional information explaining why each port state was assigned.
- Examples include `syn-ack ttl 64` for open ports and `reset ttl 64` for closed ports.

Key Observation:

- The `--reason` flag provides valuable insight into Nmap's decision-making process and helps analysts better understand scan results and target behaviour.
---

# Challenges Encountered

The most challenging aspect of this room was understanding how different TCP flag combinations influence responses from target systems. Many scans appear similar at first glance, but each technique serves a different purpose and relies on specific TCP behaviours.

It was also important to understand that advanced scans do not always identify open ports directly. Some techniques are primarily designed to gather information about firewalls, filtering mechanisms, or operating system behaviour rather than service enumeration.

---

# What I Learned

This room significantly expanded my understanding of Nmap beyond traditional port scanning. I learned that different TCP flag combinations can be used to gather information about open ports, firewall rules, and packet filtering behaviour.

I also learned how techniques such as fragmentation, decoy scanning, and idle scanning can increase stealth and complicate attribution. Most importantly, I gained a deeper appreciation for how network protocols behave under unusual conditions and how security professionals leverage those behaviours during reconnaissance.

---

# Reflection

This room demonstrated that effective reconnaissance is not simply about identifying open ports. Advanced scanning techniques provide valuable insights into how a target network is configured, how its security controls operate, and how systems respond to unexpected traffic.

Learning these techniques strengthened both my understanding of TCP/IP networking and my ability to interpret scan results. These concepts are highly relevant to penetration testing, network security assessments, and defensive monitoring, where understanding scan behaviour can help identify both attackers and vulnerabilities within a network.

