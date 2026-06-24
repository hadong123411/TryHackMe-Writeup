# Passive Reconnaissance

## Room Information

- Path: Network Reconnaissance
- Room: Passive Reconnaissance
- Platform: TryHackMe

## Objective

Learn how passive reconnaissance gathers information about a target without directly interacting with it.

## Key Concepts Learned

### Reconnaissance

Reconnaissance (Recon) is the first phase of an attack. It involves gathering information about a target before attempting exploitation.

**Two types exist**:

- Passive Reconnaissance
- Active Reconnaissance

### Passive Reconnaissance

Passive reconnaissance gathers information without directly interacting with the target.

**Examples**:

- WHOIS lookups
- DNS record enumeration
- Certificate Transparency logs
- Shodan searches
- Public GitHub repositories
- Social media and OSINT sources

**Benefits**:

- Stealthy
- Difficult to detect
- Lower legal risk

### Active Reconnaissance

Active reconnaissance involves direct interaction with the target.

**Examples**:

- Port scanning
- Service enumeration
- Directory brute forcing
- Social engineering

**Risks**:

- Can trigger IDS/IPS alerts
- Can be logged
- Requires authorization

## Tools Covered

### WHOIS

**Purpose**: Retrieve domain registration information.

**Useful Information**:

- **Registrar**: The company that mamanges the domain registration (e.g, GoDaddy, Namecheap). It is useful for identifying where the domain is hosted and understanding the organisation's infrastructure choices.
- **Registration dates**: Shows when the domain was created, updated, and expires.This can help attackers estimate how long a company has existed and identify renewal periods that attackers may target with phishing campaigns.
- **Name servers**: The DNS servers responsible for translating the domain name into IP addresses. Useful for understanding DNS infrastructure and identifying additional assets.
- **Abuse contacts**: Contact information for reporting malicious activity, phishing, spam, or security incidents related to the domain.

**Example**:

```bash
whois tryhackme.com
```

---

### DNS Enumeration

#### nslookup

**Purpose**: Query DNS records for a domain
**Why use it**:

- Allows analysts to identify the IP addressses associated with a domain
- Can reveal mail servers (MX records) and verification records (TXT records)
- Useful during reconnaissance to map a target's infrastructure
- Commonly available on both Windows and Linux systems.

**Example**:

```bash
nslookup tryhackme.com
```

**DNS Record Types**:

| Record | Purpose |
|----------|----------|
| A | IPv4 Address |
| AAAA | IPv6 Address |
| MX | Mail Servers |
| TXT | Verification/SPF/DMARC |
| CNAME | Alias |
| SOA | Start of Authority |

#### dig

**Purpose**: Advanced DNS lookup and troubleshooting tool

**Why it is useful**

- Provides more detailed DNS information than nslookup
- Displays TTL values and additional DNS response data
- Better suited for scripting and automation
- Commonly used by penetration testers and system administrator for DNS enumeration.

**Example**:

```bash
dig tryhackme.com MX
```

**Advantages**:

- Cleaner output
- Shows TTL
- Better for scripting

### DNSDumpster

**Purpose**: Passive subdomain discovery and DNS intelligence gathering.

**Why it is useful**:

- Helps identify subdomains that may not appear in standard DNS lookups.
- Reveals additional assets that could expand the attack surface
- Can uncover forgotten, outdated, or misconfigured services.
- Provides a visual map of DNS relationship and infrastructure.
- 
**Can reveal**:

- Subdomains
- IP addresses
- MX records
- TXT records

**Example findings**:

- admin.domain.com
- auth.domain.com
- assets.domain.com

---

### Certificate Transparency Logs

**Website**:

https://crt.sh

**Purpose**:

- Discover subdomains through SSL/TLS certificates.

**Example query**:

```text
%.tryhackme.com
```

**Benefits**:

- Fully passive
- Real-time certificate data

---

### Shodan

**Purpose**:

- Search engine for internet-facing devices.

**Can reveal**:

- Open ports
- Service banners
- Hosting provider
- Geographic location
- Vulnerabilities

**Example searches**:

```text
hostname:tryhackme.com
```

```text
port:443 country:US
```

---

## Commands Learned

```bash
whois tryhackme.com

nslookup tryhackme.com

nslookup -type=MX tryhackme.com

dig tryhackme.com A

dig @1.1.1.1 tryhackme.com MX
```

---

## Notes

### Things I Found Interesting

- WHOIS is gradually being replaced by RDAP.
- Certificate Transparency logs are one of the most effective passive recon techniques.
- Shodan can reveal exposed internet-facing devices without directly interacting with the target.
- Passive reconnaissance is often safer and harder to detect than active reconnaissance.

### What I Learned

This room introduced me to passive reconnaissance and its role in the reconnaissance phase of cybersecurity assessments. I learned that passive reconnaissance focuses on gathering information without directly interacting with the target, reducing the chance of detection.

I learned how WHOIS records can provide information about domain ownership, registration dates, name servers, and registrar details. This information can help analysts understand the infrastructure behind a domain and identify potential areas for further investigation.

I also explored DNS enumeration using tools such as `nslookup` and `dig`. These tools can reveal important DNS records including A, AAAA, MX, TXT, CNAME, and SOA records, which provide insights into a target's network and services.

Additionally, I learned how DNSDumpster and Certificate Transparency logs can be used to discover subdomains through publicly available data sources. These methods allow analysts to identify additional assets that may not be visible through standard DNS lookups.


### Challenges Faced

One challenge I encountered was understanding the different DNS record types and how they contribute to reconnaissance activities. While the concepts were initially unfamiliar, researching their purposes helped me understand how organisations structure their DNS infrastructure.

I also found DNSDumpster slightly difficult to navigate at first due to the amount of information presented. It took some time to understand how the discovered subdomains, IP addresses, and DNS records were related to one another.

Another challenge was exploring Shodan. Because I was connected to the Curtin University network, access to Shodan was restricted, preventing me from fully completing the practical exploration of the platform.


### Reflection

This room provided a strong foundation for understanding passive reconnaissance techniques. Before completing the room, I understood reconnaissance as simply gathering information, but I now have a clearer understanding of the distinction between passive and active reconnaissance methods.

Completing this room also reinforced the importance of publicly available information in cybersecurity. Even without interacting with a target directly, a significant amount of intelligence can be gathered through WHOIS records, DNS data, Certificate Transparency logs, and internet-wide search engines.

Overall, this room improved both my technical understanding and my confidence in using reconnaissance tools, providing a solid foundation for future penetration testing and security analysis activities.

## Screenshots

### WHOIS Lookup

<img width="699" height="327" alt="image" src="https://github.com/user-attachments/assets/60015dfa-9d6b-48f7-9516-29bb1f8cc349" />

### DNS Enumeration (TXT Records)

<img width="706" height="326" alt="image" src="https://github.com/user-attachments/assets/4941c49a-1c85-4b86-a196-26099e2cb69d" />


### Screenshot 3 - Certificate Transparency Search

<img width="1138" height="733" alt="image" src="https://github.com/user-attachments/assets/13cd475d-d8ea-4fc8-8ca2-b7997b7fc313" />


## Real-World Relevance

**As a penetration tester**:

- Passive recon helps identify attack surface before scanning.

**As a SOC Analyst**:

- Monitor CT logs, Shodan, and DNS records to identify exposed assets and unauthorized infrastructure.


## Key Takeaways

- Reconnaissance is the first phase of an engagement.
- Passive reconnaissance does not directly interact with the target.
- WHOIS, DNS, CT logs, DNSDumpster, and Shodan are common passive recon tools.
- Certificate Transparency logs are powerful for discovering subdomains.
- Shodan helps identify exposed internet-facing services.
