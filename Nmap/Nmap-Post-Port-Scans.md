# TryHackMe Write-up: Nmap Post Port Scans

## Overview

This room focused on techniques used after identifying open ports during reconnaissance. While port scanning reveals which services are accessible, post-port scanning gathers deeper intelligence about those services, including software versions, operating systems, network paths, and additional information through automated scripting.

The room explored service detection, operating system fingerprinting, traceroute functionality, the Nmap Scripting Engine (NSE), and methods for exporting scan results for documentation and future analysis.

---

## Objectives

* Identify services running on open ports.
* Determine software versions and product information.
* Perform operating system detection.
* Understand traceroute functionality.
* Explore the Nmap Scripting Engine (NSE).
* Learn how to save scan results in different formats.
* Develop more advanced reconnaissance and enumeration skills.

---

# Why Post-Port Scanning Matters

Discovering open ports is only the first stage of reconnaissance.

For example:

```bash
22/tcp open ssh
80/tcp open http
```

This information tells us which ports are open but does not reveal:

* Which software is running.
* Software versions.
* Operating system information.
* Potential vulnerabilities.
* Additional service capabilities.

Post-port scanning transforms basic port information into actionable intelligence.

---

# Service Detection

Service detection identifies the application running behind an open port and attempts to determine the software version.

Example:

```bash
nmap -sV target
```

A lighter version detection scan can also be performed:

```bash
nmap -sV --version-light target
```

Information discovered may include:

* Product name
* Version number
* Service banners
* Host information

Example output:

```text
22/tcp open ssh OpenSSH 9.2p1
80/tcp open http nginx 1.22.1
```

## Advantages

* Identifies software versions.
* Assists vulnerability assessments.
* Supports asset management.
* Provides more context than a simple port scan.

## Disadvantages

* Generates additional network traffic.
* May trigger monitoring systems.
* Version detection is not always accurate.

---

# Operating System Detection

Operating system detection attempts to identify the target's operating system by analyzing network responses and TCP/IP behaviour.

Example:

```bash
nmap -O target
```

Nmap compares packet responses against a database of known operating system fingerprints.

Possible detections include:

* Linux
* Windows
* FreeBSD
* Embedded systems
* Network appliances

## Why OS Detection Is Useful

Understanding the operating system helps security professionals:

* Prioritize vulnerabilities.
* Select appropriate tools.
* Improve asset visibility.
* Understand network architecture.

## Advantages

* Valuable reconnaissance information.
* Helps focus further enumeration.

## Disadvantages

* Can be affected by firewalls.
* Results are not always exact.
* Some systems intentionally obscure fingerprints.

---

# Traceroute

Traceroute identifies the network path between the scanner and target.

Example:

```bash
nmap --traceroute target
```

Traceroute can reveal:

* Routers
* Gateways
* Firewalls
* Network boundaries

## Why Traceroute Is Useful

Traceroute helps analysts:

* Understand network architecture.
* Identify filtering points.
* Troubleshoot connectivity issues.
* Visualize routing paths.

## Advantages

* Provides network visibility.
* Supports troubleshooting.
* Assists infrastructure mapping.

## Disadvantages

* Some devices block traceroute traffic.
* Routes may change dynamically.
* Results may be incomplete.

---

# Nmap Scripting Engine (NSE)

The Nmap Scripting Engine extends Nmap's capabilities beyond traditional port scanning.

Example:

```bash
nmap -sC target
```

NSE can automate:

* Information gathering
* Service enumeration
* Vulnerability detection
* Authentication testing
* Security auditing

This makes NSE one of the most powerful features of Nmap.

---

# NSE Script Categories

The Nmap Scripting Engine organizes scripts into categories based on their purpose.

| Category  | Purpose                                               |
| --------- | ----------------------------------------------------- |
| auth      | Authentication-related checks and credential testing  |
| broadcast | Discovers hosts and services using broadcast requests |
| brute     | Performs brute-force password attacks                 |
| default   | Scripts automatically executed with `-sC`             |
| discovery | Collects information about hosts and services         |
| dos       | Tests for Denial-of-Service vulnerabilities           |
| exploit   | Attempts exploitation of known vulnerabilities        |
| external  | Uses third-party resources and databases              |
| fuzzer    | Sends malformed input to identify weaknesses          |
| intrusive | Performs aggressive checks that may impact services   |
| malware   | Detects malware infections or suspicious activity     |
| safe      | Performs low-risk, non-intrusive checks               |
| version   | Assists with service and version detection            |
| vuln      | Searches for known vulnerabilities                    |

Example:

```bash
nmap --script vuln target
```

This executes vulnerability-focused NSE scripts against the target.

---

# Common NSE Usage

Default scripts:

```bash
nmap -sC target
```

Specific script:

```bash
nmap --script ssh2-enum-algos target
```

The room demonstrated how NSE scripts can gather information such as:

* SSH host keys
* SSL certificates
* Supported protocols
* Service capabilities
* Cryptographic algorithms

---

# Aggressive Scanning

Nmap can combine multiple techniques into a single scan.

Example:

```bash
nmap -A target
```

This enables:

* Service detection
* OS detection
* NSE scripts
* Traceroute

## Advantages

* Fast information gathering.
* Comprehensive reconnaissance.

## Disadvantages

* Generates significant traffic.
* Easy to detect.
* Can trigger alerts.

---

# Saving Scan Results

Professional assessments often require scan results to be saved for documentation and analysis.

---

## Normal Output

```bash
nmap -oN results.txt target
```

Human-readable format.

---

## XML Output

```bash
nmap -oX results.xml target
```

Useful for:

* Automation
* Reporting tools
* Parsing scan data

---

## Grepable Output

```bash
nmap -oG results.gnmap target
```

Designed for searching and filtering results.

### Advantages

* Easy automation.
* Fast filtering.

### Disadvantages

* Less human-readable.

---

## Script Kiddie Output

```bash
nmap -oS results.txt target
```

Produces stylized output intended primarily for entertainment.

---

## Save All Formats

```bash
nmap -oA scan_results target
```

Creates:

```text
scan_results.nmap
scan_results.xml
scan_results.gnmap
```

This is commonly used during professional security assessments.

---

# Skills Learned

* Service version detection.
* Operating system fingerprinting.
* Network path analysis using traceroute.
* Using the Nmap Scripting Engine.
* Understanding NSE script categories.
* Enumerating services with automated scripts.
* Exporting scan results in multiple formats.
* Organizing reconnaissance data for analysis.
* Understanding how enumeration builds upon port scanning.

---

# Screenshot Section

## Screenshot 1 – Service Detection (-sV)

<img width="605" height="316" alt="image" src="https://github.com/user-attachments/assets/b2c9dcf0-dd71-4ec8-b455-e3f087e00d6b" />

What it shows:

* Nmap service version detection using:

```bash
nmap -sV --version-light target
```

* Identification of services running on open ports.
* Detection of software versions including:

  * OpenSSH 9.2p1
  * nginx 1.22.1
  * Postfix smtpd
  * Dovecot POP3/IMAP

Key Observation:

* Service detection provides significantly more information than a basic port scan and can reveal software versions that may be vulnerable to known exploits.

---

## Screenshot 2 – Operating System Detection (-O)

<img width="611" height="471" alt="image" src="https://github.com/user-attachments/assets/814a2d64-2056-4b68-9776-3768450610b2" />

What it shows:

* Nmap attempting to fingerprint the target operating system.
* Collection of TCP/IP fingerprint data.
* Identification of network distance (1 hop).
* Analysis of packet responses to estimate the underlying operating system.

Key Observation:

* Although Nmap could not determine the exact operating system, it gathered a detailed TCP fingerprint that can be compared against known operating system signatures.

---

## Screenshot 3 – Nmap Scripting Engine Default Scripts (-sC)

<img width="614" height="587" alt="image" src="https://github.com/user-attachments/assets/b5948fd0-6a23-49ae-8e86-9eb00f14cc73" />

What it shows:

* Execution of default NSE scripts.
* Enumeration of SSH host keys.
* SMTP capabilities discovery.
* SSL certificate information.
* POP3 and IMAP capability enumeration.
* RPC service information gathering.

Key Observation:

* NSE scripts automate service enumeration and provide valuable intelligence that would otherwise require manual investigation.

---

## Screenshot 4 – NSE Script Execution (ssh2-enum-algos)

<img width="611" height="593" alt="image" src="https://github.com/user-attachments/assets/dd52ce3d-939a-4d74-85a3-646ec2ecfff2" />

What it shows:

* Execution of the `ssh2-enum-algos` NSE script.
* Enumeration of supported SSH algorithms.
* Discovery of key exchange algorithms, encryption methods, and cryptographic configurations.

Key Observation:

* Security analysts can use NSE scripts to identify weak or deprecated cryptographic algorithms that may pose security risks.

---

# Challenges Encountered

One challenge during this room was understanding the distinction between basic port scanning and post-port scanning. Initially, identifying open ports seemed sufficient; however, I learned that open ports only indicate where services are listening and do not provide meaningful information about the underlying applications.

Understanding how service detection, operating system fingerprinting, traceroute, and NSE scripts work together helped demonstrate how reconnaissance evolves from simple discovery into detailed enumeration.

---

# What I Learned

This room demonstrated that successful reconnaissance extends beyond identifying open ports. By using service detection, OS fingerprinting, traceroute, and NSE scripts, security professionals can build a detailed understanding of target systems and network infrastructure.

I also learned the importance of documenting scan results using various output formats, allowing future analysis, automation, reporting, and evidence collection during security assessments.

---

# Reflection

This room expanded my understanding of Nmap from a simple port scanning tool into a comprehensive reconnaissance platform. I learned how service detection, operating system fingerprinting, traceroute, and scripting can work together to provide detailed intelligence about a target environment.

The Nmap Scripting Engine was particularly interesting because it demonstrated how automation can significantly enhance reconnaissance efforts. Overall, this room strengthened my understanding of enumeration techniques and reinforced the importance of gathering detailed information before moving into vulnerability assessment or exploitation activities.


