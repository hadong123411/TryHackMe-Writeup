# Active Reconnaissance

## Room Information

- **Path:** Network Reconnaissance
- **Room:** Active Reconnaissance
- **Platform:** TryHackMe

## Objective

Learn how active reconnaissance gathers information by directly interacting with a target system. Unlike passive reconnaissance, these activities can be detected through logs, firewalls, or intrusion detection systems.

# Key Concepts Learned

## Active Reconnaissance

Active reconnaissance involves sending traffic directly to a target to gather information about its systems, services, and network infrastructure.

**Examples include**:

- Web browsing and Developer Tools
- Ping
- Traceroute
- Telnet
- Netcat

## Browser Reconnaissance

### Developer Tools

**Purpose:** Gather information about web applications through direct interaction.

**Useful Information:**

* HTTP headers
* Cookies
* JavaScript files
* API endpoints
* Server technologies
* Certificate details

### Browser Extensions

| Tool                | Purpose                                 |
| ------------------- | --------------------------------------- |
| Wappalyzer          | Identify technologies used by a website |
| FoxyProxy           | Manage proxy configurations             |
| User-Agent Switcher | Emulate different browsers and devices  |

## Ping

**Purpose**: Verify whether a host is online and reachable using ICMP Echo Requests.

**Key Information Revealed**:

- Host availability
- Network latency
- Packet loss
- TTL values

**Why It Matters**

Ping is often the first step in reconnaissance because it quickly confirms whether a target is reachable before performing further enumeration.

**Example**

```bash
ping -c 5 tryhackme.com
```

## Traceroute

**Purpose**: Map the network path between the source and destination.

**Why use it**

It helps reveal intermediate routers, latency at each hop, and potential points of failures or filtering.

**Key Information Revealed**

* Intermediate routers (hops)
* Network path
* Latency at each hop
* Potential filtering points

**How It Works**

Traceroute uses increasing TTL values. When a packet's TTL reaches zero, routers return an ICMP Time Exceeded message, revealing their position in the path.

**Example**

```bash
traceroute tryhackme.com
```

## Telnet

**Purpose**: Connect directly to TCP services and perform banner grabbing.

### Banner Grabbing

**Purpose**: Banner grabbing is the process of connecting to a service to identify its software, version and configuration information from the response it provides.

**Banner grabbing allows an attacker or analyst to identify**:

- Service names
- Software versions
- Configuration details

**Example**

```bash
telnet TARGET_IP 80
```

**Security Note**

Telnet sends data in plaintext and is considered insecure for remote administration. SSH is the preferred alternative.

## Netcat (nc)

**Purpose**: A versatile networking tool used for:

* Banner grabbing
* Port probing
* File transfers
* Client/server communication

**Banner Grabbing Example**

```bash
nc TARGET_IP 80
```

**Listening Example**

```bash
nc -lvnp 1234
```

**Useful Flags**

| Flag | Purpose                         |
| ---- | ------------------------------- |
| -l   | Listen mode                     |
| -p   | Specify port                    |
| -n   | Disable DNS resolution          |
| -v   | Verbose output                  |
| -vv  | Very verbose output             |
| -k   | Keep listening after disconnect |

# Skills Demonstrated

* Host discovery using ICMP
* Network path analysis using Traceroute
* Service identification through banner grabbing
* Web application reconnaissance using Developer Tools
* Technology fingerprinting using browser extensions
* Basic service enumeration using Telnet and Netcat

# Screenshots

## Ping Host Discovery

**Description:** Verified that a target host was reachable using ICMP Echo Requests.

**Evidence Captured:**

* Successful replies
* Round-trip times
* TTL values
* Packet loss statistics

<img width="521" height="297" alt="image" src="https://github.com/user-attachments/assets/d35e925a-8895-4d4c-b338-c57f7210fddc" />

## Screenshot 3 – Traceroute Analysis

**Description:** Mapped the route between the source system and the destination host.

**Evidence Captured:**

* Router hops
* Network path
* Latency measurements
* Destination reached successfully

<img width="725" height="79" alt="image" src="https://github.com/user-attachments/assets/104857ad-58e7-4c5d-b107-605febf123e3" />


## Screenshot 4 – Telnet Banner Grabbing

**Description:** Connected directly to a service and retrieved the server banner.

**Evidence Captured:**

* Service name
* Software version
* HTTP response headers

<img width="559" height="302" alt="image" src="https://github.com/user-attachments/assets/ed01df9f-d112-4fd7-b026-bbbaba75cede" />


## Screenshot 5 – Netcat Banner Grabbing

**Description:** Used Netcat to connect to a service and retrieve banner information.

**Evidence Captured:**

* Server response
* Service identification
* Software version information

**Screenshot:**

<img width="730" height="55" alt="image" src="https://github.com/user-attachments/assets/54807694-dbd1-492c-a1e8-771e9e505f71" />


# What I Learned

This room taught me how active reconnaissance differs from passive reconnaissance by directly interacting with a target. I learned how Ping confirms host availability, how Traceroute maps network paths using TTL values, and how Telnet and Netcat can be used to identify running services through banner grabbing. I also explored how browser Developer Tools can reveal valuable information about web applications, including technologies, headers, and exposed resources.

# Challenges Encountered

Understanding how Traceroute uses TTL values to discover intermediate routers initially required additional research. Interpreting route changes between multiple traceroute executions was also challenging, as network paths can vary due to load balancing and dynamic routing.

# Reflection

This room demonstrated that valuable reconnaissance can be performed using simple built-in tools before moving on to automated scanners such as Nmap. Understanding these fundamentals provides a stronger foundation for network enumeration and helps explain how more advanced reconnaissance tools operate behind the scenes.
