# Insecure Direct Object Reference (IDOR)

**Platform:** TryHackMe  
**Category:** Web Application Security  
**OWASP Category:** A01:2021 - Broken Access Control  
**Difficulty:** Easy  
**Estimated Time:** 30 Minutes  
**Status:**  Completed

---

# Overview

The **IDOR (Insecure Direct Object Reference)** room introduces one of the most common web application vulnerabilities found in modern applications. An IDOR vulnerability occurs when a web application exposes direct references to internal objects, such as user IDs, filenames, account numbers or database records, without properly verifying whether the authenticated user is authorised to access them.

Unlike vulnerabilities such as SQL Injection or Cross-Site Scripting, IDOR does not rely on injecting malicious payloads. Instead, attackers manipulate identifiers already used by the application to access resources that belong to other users.

Throughout this room, I learned how to identify insecure object references, analyse application requests, understand how encoded or hashed identifiers can still be vulnerable, and recognise why proper server-side authorisation is the only effective defence against IDOR attacks.

---

# Objectives

- Understand what an IDOR vulnerability is.
- Learn why Broken Access Control is considered the most critical OWASP vulnerability.
- Identify direct object references within web applications.
- Understand how encoded and hashed identifiers can still be vulnerable.
- Learn common locations where IDOR vulnerabilities occur.
- Complete a practical IDOR exploitation exercise.
- Understand best practices for preventing IDOR vulnerabilities.

---

# Tools Used

- Web Browser
- Firefox Developer Tools
- Network Inspector
- HTTP Requests
- JSON Response Analysis
- TryHackMe AttackBox

---

# Module Notes

## What is an IDOR?

**IDOR (Insecure Direct Object Reference)** is a type of **Broken Access Control** vulnerability where an application exposes internal object identifiers and fails to verify whether the current user is authorised to access those resources.

Instead of attacking the application through code injection, an attacker simply modifies an identifier already being used by the application.

Examples include changing:

```
/profile?id=5
```

to

```
/profile?id=6
```

If the application returns another user's profile without verifying ownership, the application is vulnerable to IDOR.

---

## Why IDOR is Dangerous

IDOR vulnerabilities can expose highly sensitive information without requiring sophisticated attack techniques.

Potential impacts include:

- Accessing another user's personal information
- Viewing confidential business records
- Downloading private documents
- Modifying another user's account
- Deleting resources
- Escalating privileges
- Performing actions on behalf of other users

Because the attacker is using legitimate application functionality, IDOR attacks often generate little suspicion.

---

## Broken Access Control

The room introduced IDOR as part of **Broken Access Control**, currently ranked as **OWASP Top 10: A01 (2021)**.

Access control determines **what authenticated users are allowed to access**.

A secure application should always verify:

- Who the user is.
- Which resources belong to them.
- Whether they have permission to perform the requested action.

Authentication answers:

> "Who are you?"

Authorisation answers:

> "What are you allowed to access?"

Many developers correctly implement authentication but neglect authorisation checks, leading to IDOR vulnerabilities.

---

## Direct Object References

Applications frequently reference internal resources using identifiers.

Common examples include:

```
/user/15

/order/1042

/document?id=25

/invoice/2025

/profile?userid=8
```

These identifiers often correspond directly to records stored within the application's database.

If the application trusts user-supplied identifiers without verifying ownership, attackers can simply replace one identifier with another.

---

## Horizontal vs Vertical Access Control

One important concept introduced in this room was the distinction between **horizontal** and **vertical** privilege escalation.

### Horizontal Privilege Escalation

Occurs when one user accesses another user at the same privilege level.

Example:

```
Alice → Bob's profile
```

Both users are ordinary users, but Alice should never access Bob's data.

This is the most common form of IDOR.

---

### Vertical Privilege Escalation

Occurs when a lower-privileged user gains access to higher-privileged resources.

Example:

```
User → Administrator
```

This may allow attackers to:

- Modify administrator accounts
- View management dashboards
- Change system settings
- Delete application data

Vertical privilege escalation typically has a much greater security impact.

---

## Finding IDORs in Encoded IDs

Developers sometimes attempt to hide identifiers by encoding them.

Example:

```
User ID:

25
```

Encoded:

```
MjU=
```

(Base64)

Although the identifier appears different, encoding does **not** provide security because it can easily be reversed.

Attackers commonly decode values using:

- Burp Decoder
- CyberChef
- Base64 utilities
- Browser Developer Tools

After decoding the identifier, attackers simply modify it before re-encoding and resubmitting the request.

This demonstrates that **encoding is not access control**.

---

## Finding IDORs in Hashed IDs

Some applications replace identifiers with cryptographic hashes.

Example:

```
5

↓

ef2d127de37b...
```

At first glance this appears secure.

However, hashing alone does not prevent IDOR vulnerabilities if:

- Hashes are predictable.
- Hashes correspond directly to sequential IDs.
- The server never verifies ownership.

Even when identifiers are difficult to predict, the server must still verify that the authenticated user has permission to access the requested resource.

Hashing improves obscurity but **does not replace authorisation**.

---

## Finding IDORs in Unpredictable IDs

Modern applications frequently replace sequential IDs with randomly generated identifiers such as UUIDs.

Example:

```
8d31ec91-f2cf-46d4-b2f3-d7713e9f9ad1
```

These identifiers are significantly more difficult to guess.

However, unpredictable identifiers alone are **not sufficient protection**.

If an attacker somehow discovers another valid UUID and the application fails to verify ownership, the IDOR vulnerability still exists.

This reinforces one of the room's most important lessons:

> **Security should never rely solely on hiding object identifiers.**

Proper server-side authorisation checks remain essential regardless of how identifiers are generated.


# Where are IDOR Vulnerabilities Located?

One of the key lessons from this room is that IDOR vulnerabilities are **not limited to URLs**. They can exist anywhere an application references an internal object without verifying whether the authenticated user is authorised to access it.

During a web application assessment, testers should inspect every location where object identifiers are transmitted between the client and server.

Common locations include:

### URL Parameters

```
/profile?id=15
```

```
/invoice/104
```

```
/download?file=report.pdf
```

Simply changing the identifier may reveal another user's information if proper authorisation checks are missing.

---

### API Endpoints

Modern web applications frequently communicate through APIs that return JSON data.

Example:

```http
GET /api/customers/15
```

Response:

```json
{
  "id": 15,
  "username": "john911",
  "email": "j@fakemail.thm"
}
```

If the application returns another user's information after changing the object ID, an IDOR vulnerability exists.

API testing is therefore an essential component of modern web application security assessments.

---

### HTTP POST Requests

Identifiers are not always located within URLs.

Applications may transmit object references inside POST requests.

Example:

```http
POST /updateProfile
```

```
userid=15
email=user@email.com
```

Changing the `userid` parameter may allow attackers to modify another user's information.

---

### JSON Requests

Modern REST APIs often exchange JSON objects.

Example:

```json
{
    "userid":15,
    "role":"user"
}
```

Changing the value may expose another user's account if the server trusts client-supplied identifiers.

---

### Cookies

Some poorly designed applications store object identifiers inside cookies.

Example:

```
userid=25
```

Changing the cookie value could potentially grant access to another account if server-side authorisation is absent.

---

### Hidden HTML Fields

Developers sometimes assume hidden form fields are secure because they are not visible on the webpage.

Example:

```html
<input type="hidden"
name="userid"
value="15">
```

Since browsers allow users to modify hidden fields, they should never be trusted for authorisation decisions.

---

### AJAX Requests

One of the practical examples in this room demonstrated that applications frequently retrieve data using **AJAX (Asynchronous JavaScript and XML)** requests.

Unlike traditional page loads, AJAX communicates with the server in the background without refreshing the webpage.

Inspecting these requests through the browser's Developer Tools often reveals API endpoints that expose object identifiers.

For penetration testers, analysing background network traffic is an important step when searching for IDOR vulnerabilities.

---

# Practical Exercise Summary

The practical lab focused on identifying insecure object references by analysing HTTP requests generated by a vulnerable web application.

Using Firefox Developer Tools, I inspected the application's background network traffic and identified an AJAX request returning customer information in JSON format.

The response exposed internal object identifiers that could be manipulated to retrieve information belonging to other users.

This exercise demonstrated that sensitive information is not always exposed through visible webpages; instead, it is often accessible through backend API requests.

Rather than exploiting complex vulnerabilities, the attack simply involved modifying existing identifiers and observing whether the application enforced proper authorisation.

This reinforced the principle that Broken Access Control vulnerabilities frequently arise from insecure business logic rather than programming errors.

---

# What I Learned

- The difference between authentication and authorisation.
- Why Broken Access Control is currently ranked as **OWASP Top 10 A01 (2021)**.
- How IDOR vulnerabilities occur when applications trust client-supplied object identifiers.
- Why sequential identifiers significantly increase the likelihood of IDOR vulnerabilities.
- That Base64 encoding does not provide security because encoded values can easily be decoded and modified.
- Why hashing object identifiers alone does not prevent unauthorised access.
- How UUIDs improve unpredictability but still require proper authorisation checks.
- Where IDOR vulnerabilities commonly appear within web applications.
- How browser Developer Tools can reveal hidden API requests that expose sensitive object references.
- Why every request involving object identifiers should be validated on the server.

---

# What I Found Interesting

The most interesting concept introduced in this room was that **IDOR vulnerabilities do not require sophisticated exploitation techniques**.

Unlike SQL Injection or Cross-Site Scripting, attackers often do not need to inject malicious code or bypass application security mechanisms. Instead, they simply modify identifiers that already exist within legitimate requests.

I also found it interesting that encoding and hashing are often incorrectly viewed as security controls. Before completing this room, it was easy to assume that hiding identifiers behind Base64 encoding or cryptographic hashes would prevent unauthorised access. However, the room demonstrated that these techniques merely obscure identifiers rather than enforce access control.

Another interesting takeaway was the importance of analysing background API traffic. Many modern applications rely heavily on asynchronous requests that users never directly see. Using browser Developer Tools to inspect these requests revealed how much sensitive information can be exposed without appearing on the visible webpage.

Finally, learning that Broken Access Control is currently the **highest-ranked OWASP vulnerability** reinforced how common and impactful these flaws remain despite being conceptually straightforward to prevent.

---

# Practical Security Recommendations

Based on the concepts covered in this room, applications should implement the following security practices:

- Verify authorisation on every request.
- Never trust user-controlled object identifiers.
- Avoid exposing sequential identifiers where possible.
- Implement Role-Based Access Control (RBAC).
- Validate resource ownership server-side.
- Use UUIDs to reduce identifier predictability.
- Log unauthorised access attempts.
- Regularly perform access control testing during security assessments.

Although UUIDs and hashed identifiers improve security through obscurity, they should always be combined with proper server-side authorisation checks.

# Challenges Encountered

### Understanding the Difference Between Authentication and Authorisation

One of the biggest conceptual challenges during this room was understanding the distinction between **authentication** and **authorisation**.

Initially, both concepts appeared similar because they both relate to user accounts and permissions. However, the practical exercises demonstrated that they serve entirely different purposes.

- **Authentication** verifies a user's identity.
- **Authorisation** determines what resources that authenticated user is allowed to access.

This distinction became much clearer after observing how a legitimate authenticated account could still access another user's information due to missing server-side authorisation checks.

---

### Recognising Hidden Object References

Another challenge was identifying where object identifiers were located within the application.

Rather than appearing directly in the webpage URL, identifiers were embedded within background API requests and JSON responses. This required careful inspection using the browser's Developer Tools and Network Inspector.

This exercise reinforced the importance of analysing all network traffic during web application assessments instead of focusing solely on visible webpages.

---

### Understanding Why Encoding Is Not Security

Initially, it seemed logical that encoding identifiers would provide some level of protection.

However, after learning about Base64 encoding and observing how encoded identifiers could easily be decoded, modified and re-encoded, it became clear that encoding only obscures information rather than enforcing security.

This highlighted an important lesson:

> **Security through obscurity should never replace proper access control.**

---

### Understanding Modern Web Applications

Many modern web applications rely heavily on APIs and asynchronous requests instead of traditional page loads.

Initially, it was easy to overlook these background requests because they occur silently behind the scenes.

Using the Network Inspector demonstrated how much application functionality is handled through API communication, reinforcing why developers and penetration testers must inspect network traffic during security assessments.

---

# Screenshot Placeholders

## Screenshot 1 — Room Completion

**Description**

Completed the TryHackMe IDOR room after successfully working through each module covering direct object references, encoded identifiers, hashed identifiers, unpredictable identifiers, and practical exploitation.

<img width="1179" height="829" alt="image" src="https://github.com/user-attachments/assets/288394a4-00e1-4787-86ef-e7967869f042" />


---

## Screenshot 2 — Practical IDOR Exercise

**Description**

Inspected the application's background AJAX request using Firefox Developer Tools and observed the JSON response containing customer information. This demonstrated how object identifiers exposed through API responses can lead to IDOR vulnerabilities when proper server-side authorisation checks are absent.

<img width="1290" height="1372" alt="image" src="https://github.com/user-attachments/assets/e1c42c42-2355-4fa8-bafe-548c9cc50f7a" />


---

# Key Takeaways

- IDOR is a form of **Broken Access Control**, currently ranked as **OWASP Top 10 A01 (2021)**.
- Authentication does not automatically guarantee proper authorisation.
- Every request accessing protected resources must be validated on the server.
- Object identifiers should never be trusted simply because a user is authenticated.
- Base64 encoding provides no security because encoded values can easily be reversed.
- Cryptographic hashes improve obscurity but do not replace authorisation checks.
- UUIDs reduce identifier predictability but still require access control validation.
- Modern web applications frequently expose sensitive object references through REST APIs and JSON responses.
- Browser Developer Tools are valuable for identifying hidden requests that may contain exploitable object identifiers.
- Broken Access Control vulnerabilities often arise from flawed application logic rather than software bugs.

---

# Overall Reflection

This room provided an excellent introduction to one of the most common and impactful web application vulnerabilities affecting modern systems. Unlike vulnerabilities that rely on injecting malicious input, IDOR demonstrated that serious security flaws can result simply from trusting user-controlled object identifiers without verifying whether the requester is authorised to access the associated resource.

The practical exercise reinforced how easily sensitive information can be exposed through APIs and background requests when proper access control mechanisms are absent. Analysing network traffic using browser Developer Tools highlighted that valuable information is often exchanged behind the scenes, making network inspection an essential skill for web application security testing.

One of the most valuable lessons from this room was understanding that **authentication and authorisation are separate security controls**. Even when a user has successfully authenticated, applications must still verify that every request is authorised before granting access to protected resources.

This room also reinforced that techniques such as Base64 encoding, hashing, or using unpredictable identifiers should never be relied upon as primary security controls. While these methods may make identifiers more difficult to guess, they cannot replace robust server-side authorisation checks.

Overall, this room significantly strengthened my understanding of Broken Access Control and highlighted why it remains the highest-ranked vulnerability in the OWASP Top 10. The concepts learned here provide an important foundation for future web application security assessments and demonstrate the importance of secure access control design in modern software development.

---

# References

- OWASP Top 10 (2021) – A01: Broken Access Control
- OWASP IDOR Prevention Cheat Sheet
- TryHackMe: IDOR
- Mozilla Firefox Developer Tools Documentation
- REST API Security Best Practices

---

# Skills Developed

## Web Application Security

- IDOR Identification
- Access Control Testing
- API Analysis
- JSON Response Inspection
- HTTP Request Analysis
- Browser Developer Tools

---

## Offensive Security

- Object Reference Manipulation
- API Endpoint Enumeration
- Network Traffic Analysis
- Manual Web Application Testing
- Vulnerability Discovery

---

## Defensive Security

- Access Control Validation
- Principle of Least Privilege
- Secure API Design
- Object-Level Authorisation
- Secure Development Practices

---

# Portfolio Reflection

This room strengthened my understanding of **Broken Access Control**, one of the most critical categories of web application vulnerabilities. While SQL Injection focuses on manipulating database queries, IDOR emphasises the importance of validating user permissions before granting access to resources.

Completing this room improved my ability to identify insecure object references, analyse API traffic, and understand how modern web applications exchange sensitive information. More importantly, it reinforced that secure application design depends not only on protecting data but also on ensuring that every request is properly authorised.

Combined with my previous documentation on Burp Suite and SQL Injection, this room further expands my practical knowledge of manual web application security testing and contributes to a stronger foundation for future penetration testing and secure software assessment.
