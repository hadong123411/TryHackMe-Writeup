# Nmap Live Host Discovery

## Overview

This room focused on identifying active hosts on a network before performing detailed reconnaissance or port scanning. Host discovery is an essential step in penetration testing and network administration because it helps determine which systems are online and worth investigating further.

The room explored various Nmap host discovery techniques, including ARP discovery, ICMP-based discovery, TCP and UDP probes, target enumeration methods, and reverse DNS lookups.

## Objectives

* Understand the purpose of host discovery.
* Learn how to identify active systems on a network.
* Explore different Nmap discovery techniques.
* Understand the advantages and limitations of each method.
* Learn how DNS information can assist reconnaissance efforts.

# Key Concepts Learned

## Why Host Discovery Matters

Before performing a port scan, security professionals need to determine which devices are actually online.

Host discovery helps:

* Reduce unnecessary scanning.
* Improve scan efficiency.
* Identify active targets.
* Minimize network noise.
* Focus reconnaissance efforts on live systems.

Instead of scanning thousands of addresses, analysts can first identify which hosts are active and then perform deeper enumeration.


## Subnetting and Network Ranges

Host discovery often begins by identifying the target network range.

**Examples include**:

```bash id="ayd0u2"
192.168.1.0/24
```

or

```bash id="j91s8o"
10.10.10.0/24
```

Subnetting allows security professionals to define which addresses belong to a particular network and determine the scope of a scan.

Understanding subnet ranges is important because it ensures that discovery scans target the correct systems.

## Target Enumeration

Nmap provides several methods for identifying potential targets before active scanning begins.

### Reading Targets from a File

Nmap can import a list of targets from a file.

Example:

```bash id="p6e8nm"
nmap -iL targets.txt
```

### Advantages

* Efficient for large target lists.
* Useful during assessments involving many systems.
* Easy to automate.

### Disadvantages

* Requires maintaining accurate target lists.
* Outdated files may contain inactive systems.

---

### List Scan

A list scan displays targets without sending packets.

Example:

```bash id="e2nzjz"
nmap -sL 10.10.10.0/24
```

### Advantages

* Generates no network traffic.
* Useful for planning scans.
* Helps verify target ranges.

### Disadvantages

* Does not determine whether hosts are alive.
* Only performs DNS-related lookups.

---

### Disable DNS Resolution

Example:

```bash id="4hx1vl"
nmap -n 10.10.10.0/24
```

### Advantages

* Faster scans.
* Reduces DNS-related delays.
* Useful when DNS information is unnecessary.

### Disadvantages

* No hostname information is returned.
* Results may be harder to interpret.

---

## ARP-Based Host Discovery

Within local networks, Nmap commonly uses ARP requests to identify active hosts.

Example:

```bash id="8u4c5g"
nmap -PR -sn 192.168.1.0/24
```

ARP works by asking:

> "Who owns this IP address?"

Active systems respond with their MAC address.

### Advantages

* Extremely reliable on local networks.
* Fast discovery method.
* Difficult for local hosts to ignore.

### Disadvantages

* Only works on local Layer 2 networks.
* Cannot discover hosts across the internet.

---

## Ping Sweep Discovery

A common host discovery technique is performing a ping sweep.

Example:

```bash id="b70ohq"
nmap -sn 10.10.10.0/24
```

The `-sn` option performs host discovery without conducting a port scan.

### Advantages

* Fast.
* Low network overhead.
* Ideal for identifying live hosts.

### Disadvantages

* Some systems block ping requests.
* Firewalls may hide active hosts.

---

## ICMP Echo Discovery

ICMP Echo Requests are commonly known as traditional ping requests.

Nmap sends ICMP Echo packets and waits for responses.

### Advantages

* Simple and widely supported.
* Fast discovery method.

### Disadvantages

* Many organizations block ICMP traffic.
* Can generate false negatives.

---

## ICMP Timestamp Requests

Nmap can also use ICMP Timestamp Requests to identify active hosts.

Unlike a traditional ping, the target responds with timing information.

### Advantages

* Can discover hosts when Echo Requests are filtered.
* Useful during reconnaissance.

### Disadvantages

* Often filtered by modern firewalls.
* Less commonly enabled today.

---

## TCP-Based Host Discovery

When ICMP is blocked, Nmap can use TCP packets to determine host availability.

Example:

```bash id="0w8m0u"
nmap -PS80,443 -sn target-network
```

The scanner sends TCP SYN packets to common ports.

If a response is received, the host is likely online.

### Advantages

* Effective against networks that block ICMP.
* Mimics legitimate traffic.

### Disadvantages

* May trigger security monitoring systems.
* Generates more traffic than ICMP.

---

## UDP-Based Host Discovery

Nmap can also use UDP probes.

Example:

```bash id="ujq6oi"
nmap -PU53 -sn target-network
```

The scanner sends UDP packets to specific ports and analyzes responses.

### Advantages

* Useful when TCP discovery is restricted.
* Can identify systems running UDP services.

### Disadvantages

* Slower than TCP discovery.
* Responses are less reliable.
* Often filtered.

## Reverse DNS Lookups

Reverse DNS attempts to resolve IP addresses into hostnames.

Example:

```bash id="m9rt2n"
nmap -R target-network
```

A hostname can provide valuable information such as:

* Device roles
* Server functions
* Department names
* Internal naming conventions

Example:

```text id="uh0hh6"
192.168.1.20 -> hr-server.company.local
```

### Advantages

* Provides context about systems.
* Assists further reconnaissance.
* Helps identify critical assets.

### Disadvantages

* Not always configured correctly.
* Can increase scan duration.


## Host Discovery Strategy

No single discovery technique works in every environment.

Security professionals often combine methods:

1. ARP discovery for local networks.
2. ICMP discovery where allowed.
3. TCP discovery when ICMP is filtered.
4. UDP discovery when necessary.
5. Reverse DNS lookups for additional context.

Using multiple techniques increases the likelihood of identifying active systems.

## Skills Learned

* Understanding subnet ranges and target networks.
* Using Nmap for host discovery.
* Reading target lists from files.
* Performing list scans.
* Conducting ARP-based discovery.
* Using ICMP Echo and Timestamp Requests.
* Performing TCP and UDP host discovery.
* Using reverse DNS lookups.
* Understanding advantages and limitations of different discovery methods.
* Planning reconnaissance activities efficiently.


# Screenshot Section

## Screenshot 1 – Basic Nmap Host Discovery and List Scan

<img width="603" height="277" alt="image" src="https://github.com/user-attachments/assets/c5d58ae7-dfd8-44fb-ae9f-6b1b6a479dc5" />

What it shows:

* Nmap discovering active hosts.
* Initial reconnaissance process.
* Identification of online systems within a target range.
* Nmap listing target addresses.
* Verification of target scope before scanning.
* DNS information associated with targets.

## ARP Discovery (-PR -sn)

<img width="602" height="148" alt="image" src="https://github.com/user-attachments/assets/62b4af4d-6120-4fff-8b9b-4af294f28d26" />

What it shows:

* ARP-based host discovery.
* Identification of live hosts on the local network.
* MAC address responses from active systems.


## ICMP Timestamp Discovery (-PP -sn)

<img width="607" height="116" alt="image" src="https://github.com/user-attachments/assets/634617be-7eb1-4a0f-8c2a-586abefce528" />

What it shows:

* ICMP Timestamp Requests being used for host discovery.
* Detection of active systems through ICMP responses.
* Alternative discovery technique when standard pings may not be available.


# Challenges Encountered

One challenge during this room was understanding the differences between the various host discovery techniques. Although several commands may appear similar, each method uses different protocols and network behaviors.

It was important to understand when ARP discovery is preferable, why ICMP may fail due to firewall restrictions, and how TCP or UDP probes can be used as alternative discovery methods. Learning the purpose behind each technique helped clarify when they should be used during real-world reconnaissance.


# What I Learned

This room demonstrated that effective reconnaissance begins with identifying active systems before performing deeper scans. I learned that Nmap supports multiple host discovery methods, each designed for different network environments and security configurations.

I also gained a better understanding of how ARP, ICMP, TCP, UDP, and DNS can all contribute to identifying live hosts. Rather than relying on a single technique, combining multiple discovery methods can improve accuracy and reduce the chances of missing important systems.

# Reflection

This room strengthened my understanding of the reconnaissance phase of a penetration test. Before this room, I primarily viewed Nmap as a port scanning tool. However, I learned that host discovery is a critical step that improves efficiency and helps focus further investigation.

Understanding the strengths and limitations of different discovery methods provided valuable insight into how security professionals map networks and identify potential targets. These skills are directly applicable to both penetration testing and defensive security operations where network visibility is essential.

<img width="509" height="49" alt="image" src="https://github.com/user-attachments/assets/77350c7a-5668-45f2-880f-fc8a33159ca8" />
