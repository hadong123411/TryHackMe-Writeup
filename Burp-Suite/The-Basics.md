# Burp Suite: The Basics

## Overview

This TryHackMe room introduced the fundamentals of Burp Suite, one of the most widely used web application security testing tools. The room focused on navigating Burp Suite, configuring browser proxies, inspecting HTTP traffic, mapping web applications, and discovering hidden content through the Site Map feature.

The room also demonstrated how Burp Suite acts as an intermediary between a browser and a web server, allowing security professionals to observe, analyse, and manipulate web traffic during testing.

---

## Learning Objectives

* Understand the purpose of Burp Suite
* Explore Burp Suite Community Edition features
* Navigate Burp Suite modules and tabs
* Configure FoxyProxy for browser integration
* Intercept and inspect HTTP requests and responses
* Use HTTP History to analyse web traffic
* Generate a Site Map through browsing
* Understand Scope and Target management
* Configure HTTPS interception
* Discover hidden endpoints using Site Map analysis

---

# What is Burp Suite?

Burp Suite is a web application security testing platform developed by PortSwigger. It functions as a proxy between a user's browser and a target web application.

```text
Browser <--> Burp Suite <--> Web Server
```

By positioning itself between the browser and server, Burp Suite can:

* Capture requests and responses
* Modify traffic before forwarding
* Analyse application behaviour
* Discover hidden content
* Assist with vulnerability testing

---

# Burp Suite Community Edition Features

Although the Community Edition has fewer capabilities than the Professional Edition, it still provides several powerful modules.

## Proxy

Captures and intercepts HTTP/S traffic between the browser and target application.

### Uses

* View requests
* View responses
* Modify traffic
* Analyse application behaviour

---

## Repeater

Allows requests to be resent multiple times with modifications.

### Uses

* Manual testing
* Parameter manipulation
* Input validation testing
* Vulnerability verification

---

## Intruder

Used for automated request spraying and fuzzing.

### Uses

* Brute-force testing
* Endpoint discovery
* Parameter fuzzing

---

## Decoder

Provides encoding and decoding functionality.

### Supports

* URL Encoding
* Base64
* HTML Encoding
* Hexadecimal Conversion

---

## Comparer

Allows comparison of two requests or responses.

### Uses

* Response analysis
* Authentication testing
* Identifying subtle differences

---

## Sequencer

Evaluates randomness within tokens.

### Uses

* Session token testing
* Cookie randomness analysis
* Entropy assessment

---

## Extensions

Burp supports community-developed extensions through the BApp Store.

Extensions can enhance functionality and automate specialised testing tasks.

---

# Burp Suite Navigation

Burp Suite is divided into modules and sub-tabs.

## Primary Navigation Bar

The top navigation bar contains major modules:

* Dashboard
* Target
* Proxy
* Intruder
* Repeater
* Sequencer
* Decoder
* Comparer
* Logger
* Extensions

---

## Secondary Navigation Bar

Each module contains additional sub-tabs.

Example:

```text
Proxy
├── Intercept
├── HTTP History
├── WebSockets History
└── Proxy Settings
```

---

## Useful Shortcuts

| Shortcut         | Function  |
| ---------------- | --------- |
| Ctrl + Shift + D | Dashboard |
| Ctrl + Shift + T | Target    |
| Ctrl + Shift + P | Proxy     |
| Ctrl + Shift + I | Intruder  |
| Ctrl + Shift + R | Repeater  |

---

# Burp Proxy

The Proxy module is one of Burp Suite's most important features.

It enables the inspection and manipulation of web traffic before it reaches the target server.

## Intercept

Intercept allows requests to be paused before being sent.

Possible actions include:

* Forward
* Drop
* Modify
* Send to Repeater
* Send to Intruder

### Why This Matters

Intercepting requests provides complete visibility into client-server communication and forms the foundation of web application testing.

---

## HTTP History

HTTP History automatically records all traffic passing through Burp.

Information recorded includes:

* Request methods
* URLs
* Status codes
* Response sizes
* Content types

This allows testers to review previously visited pages and identify hidden functionality.

---

## WebSockets History

Burp can also capture WebSocket communications.

This is useful when analysing modern web applications that rely on real-time communication.

---

# Configuring FoxyProxy

FoxyProxy was used to route browser traffic through Burp Suite.

### Configuration

| Setting | Value     |
| ------- | --------- |
| Host    | 127.0.0.1 |
| Port    | 8080      |

Once configured, browser traffic was automatically forwarded to Burp Proxy.

---

# Site Map and Issue Definitions

## Site Map

The Site Map automatically records and organises discovered application content.

### Benefits

* Application mapping
* Endpoint discovery
* Enumeration support
* API endpoint identification
* Hidden content discovery

As users browse a website, Burp automatically builds a structured tree of discovered resources.

---

## Issue Definitions

Issue Definitions acts as Burp Suite's vulnerability knowledge base.

It provides:

* Vulnerability descriptions
* Impact information
* Remediation guidance
* Reference material

This can assist during manual testing and report writing.

---

## Scope Settings

Scope settings allow testers to define which domains and resources are included in testing.

### Benefits

* Reduced noise
* Improved focus
* Better organisation
* Controlled traffic collection

---

# HTTPS Proxying

When intercepting HTTPS traffic, Burp performs TLS interception.

Browsers do not automatically trust Burp's certificate authority, so a PortSwigger CA certificate must be installed.

### Certificate Download

```text
http://burp/cert
```

After installation, Burp can decrypt and inspect HTTPS traffic securely.

---

# Practical Exercise: Hidden Endpoint Discovery

One of the room challenges involved using Burp's Site Map functionality to identify an unusual endpoint.

### Normal Endpoints

```text
/about/
/contact/
/products/
/ticket/
```

### Suspicious Endpoint

```text
/5yjR2GLcoGoij2ZK
```

Unlike the other human-readable pages, this endpoint appeared as a random string and stood out during Site Map analysis.

By selecting the endpoint and reviewing the server response, the hidden flag was revealed.

### Key Lesson

Burp Suite helps identify content that may not be immediately visible through normal browsing.

This demonstrates the importance of reconnaissance and enumeration during web application testing.

---

# Skills Learned

* Web proxy fundamentals
* Browser proxy configuration
* HTTP request analysis
* HTTP response analysis
* Traffic interception
* Web application enumeration
* Site Map analysis
* Hidden endpoint discovery
* HTTPS interception concepts
* Burp Suite navigation

---

# Screenshots

## Flag Retrieval and Burp Suite Exploration

<img width="929" height="656" alt="image" src="https://github.com/user-attachments/assets/f012c7d0-2c2f-4234-8e70-fb948bc20138" />

**Description:**
Viewing the server response associated with the hidden endpoint and retrieving the challenge flag.

---
# What I Learned

- Burp Suite acts as a proxy between a browser and a web server.
- HTTP requests and responses can be captured, inspected, and modified before reaching their destination.
- The Proxy module provides visibility into web application traffic.
- HTTP History records all traffic passing through Burp for later analysis.
- Site Maps automatically organise discovered application content into a structured hierarchy.
- Hidden pages and endpoints can often be identified through traffic analysis rather than guessing URLs.
- FoxyProxy can be used to route browser traffic through Burp Suite.
- HTTPS traffic requires a trusted PortSwigger CA certificate to enable interception.
- API endpoints expose application functionality and are commonly targeted during security assessments.
- Enumeration and reconnaissance are critical first steps when testing web applications.

# Challenges Faced

* Initial difficulty understanding how Burp Proxy interacts with browser traffic.
* Troubleshooting FoxyProxy configuration and browser integration.
* Understanding the difference between requests and responses.
* Learning how Site Map entries are generated from captured traffic.
* Identifying why the unusual endpoint was significant.

---

# Reflection

This room provided a strong introduction to Burp Suite and demonstrated how web application traffic can be inspected and analysed through a proxy. The most valuable lesson was learning how Burp Suite automatically maps web applications and reveals content that may not be immediately visible through normal browsing.

The hidden endpoint challenge reinforced the importance of reconnaissance and enumeration. Rather than guessing URLs, Burp Suite allowed me to observe application behaviour, analyse captured traffic, and identify unusual resources through the Site Map feature. These skills form a foundation for future web application security testing, vulnerability assessment, and penetration testing activities.

