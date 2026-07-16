

<img width="679" height="503" alt="image" src="https://github.com/user-attachments/assets/ded57e3c-faae-499c-9faf-337ceb212eef" />

# Burp Suite: Repeater

**Platform:** TryHackMe  
**Category:** Web Application Security  
**Difficulty:** Easy  
**Estimated Time:** 60 Minutes  
**Status:** ✅ Completed

---

# Overview

The **Burp Suite: Repeater** room introduces one of Burp Suite's core manual testing tools. Unlike Burp Proxy, which captures and forwards browser traffic, Repeater allows security analysts to duplicate an HTTP request, modify it manually, and resend it repeatedly without interacting with the browser again.

The room focuses on understanding how web applications process HTTP requests, how to inspect server responses, and how manual request manipulation can be used to verify vulnerabilities such as SQL Injection. It also introduces Burp's Inspector panel, response comparison techniques, and best practices for analysing HTTP traffic during a penetration test.

---

# Objectives

- Understand the purpose of Burp Repeater.
- Learn how HTTP requests and responses are structured.
- Perform manual request manipulation.
- Analyse application behaviour using server responses.
- Use Repeater to verify a SQL Injection vulnerability.
- Become familiar with Burp Suite's Inspector panel.

---

# Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Burp Inspector
- Web Browser
- TryHackMe AttackBox

---

# Module Notes

## What is Burp Repeater?

Burp Repeater is a **manual HTTP request editor** that allows penetration testers to resend the same request multiple times after making modifications.

Instead of repeating actions through a web browser, requests can be edited directly and resent instantly, making vulnerability testing significantly faster and more controlled.

**Common Uses**

- Manual vulnerability verification
- Testing user input validation
- Parameter manipulation
- Authentication testing
- Session analysis
- Debugging web applications
- API testing

---

## HTTP Request Structure

A request sent to a web server typically contains:

```http
GET /about/0 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Cookie: session=abc123
```

### Important Components

| Component | Description |
|-----------|-------------|
| Request Method | GET, POST, PUT, DELETE |
| URL Path | Resource requested from the server |
| Headers | Additional information about the client |
| Cookies | Session authentication information |
| Parameters | User-controlled input |
| Body | Data submitted in POST or PUT requests |

Understanding each component makes it easier to identify where user input can be manipulated during security testing.

---

## HTTP Response Structure

After receiving a request, the server returns a response containing:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

The response normally includes:

- Status Code
- Response Headers
- HTML Source
- JSON or XML Data
- Error Messages
- Server Information

### Common HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK |
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

---

## Request Replay

One of Repeater's primary features is **Request Replay**.

Instead of generating new traffic from the browser, the same request can be resent repeatedly after modifying one parameter at a time.

Typical workflow:

1. Capture a request.
2. Send it to Repeater.
3. Modify a parameter.
4. Send the request.
5. Observe the response.
6. Repeat until the application's behaviour is understood.

This technique allows security testers to isolate changes and determine how the application processes user input.

---

## Inspector Panel

The Inspector panel automatically organises HTTP messages into structured sections.

Instead of manually reading raw HTTP requests, Inspector separates information into:

- Request Headers
- Response Headers
- Query Parameters
- Request Body Parameters
- Cookies
- Request Attributes

This improves readability and reduces the chance of overlooking important information.

---

## Response Comparison

One of the most valuable penetration testing techniques introduced in this room was comparing server responses after making small changes to a request.

Useful comparison points include:

- Status codes
- Response length
- HTML content
- Error messages
- Redirect behaviour
- Database output

Even subtle differences can indicate authentication flaws, input validation issues, or injection vulnerabilities.

---

## SQL Injection Verification

The practical exercise demonstrated how Burp Repeater can manually verify a SQL Injection vulnerability.

A **UNION-based SQL Injection payload** was inserted into the request.

Example payload:

```sql
UNION ALL SELECT notes,NULL,NULL,NULL,NULL
FROM people
WHERE id = 1
```

After sending the modified request, the application returned additional database information that was not originally intended to be accessible.

The server response revealed the TryHackMe flag inside the HTML page, demonstrating how improperly validated user input can allow attackers to retrieve backend database content.

---

## Why Burp Repeater Matters

Unlike automated vulnerability scanners, Repeater provides complete control over every HTTP request.

It is commonly used to:

- Verify vulnerabilities manually.
- Experiment with payloads safely.
- Observe application behaviour.
- Reproduce findings for reporting.
- Investigate authentication and session management.
- Test APIs and custom web applications.

Because every request is manually controlled, Repeater remains one of the most important tools used during professional web application penetration testing.

---

# Practical Exercise Summary

During the lab, I modified an intercepted HTTP request using Burp Repeater by inserting a SQL Injection payload into the request parameter.

The modified request was sent directly to the server without using the web browser interface.

The application processed the injected SQL query and returned additional database information, confirming the presence of a SQL Injection vulnerability.

By analysing the HTTP response, I successfully located the hidden TryHackMe flag embedded within the returned HTML source.

This demonstrated how Burp Repeater can be used to manually validate vulnerabilities rather than relying solely on automated scanning tools.

---

# What I Learned

- How Burp Repeater differs from Burp Proxy and when each tool should be used.
- The structure of HTTP requests and responses.
- How to manually modify requests without interacting with the browser.
- How to compare server responses to identify abnormal application behaviour.
- The importance of changing one parameter at a time during testing.
- How SQL Injection vulnerabilities can be manually verified using crafted payloads.
- How Burp's Inspector panel improves efficiency when analysing HTTP traffic.
- Why manual testing remains essential even when automated scanners are available.

---

# What I Found Interesting

The most interesting part of this room was seeing how a single modified HTTP request could completely change the behaviour of the application.

Instead of interacting with the website through its graphical interface, Burp Repeater communicates directly with the web server. This highlights how attackers can bypass client-side restrictions by manipulating raw HTTP requests rather than relying on the application's front end.

I also found it interesting that Repeater is primarily a **verification tool** rather than an exploitation tool. It encourages a systematic testing methodology where each modification is analysed individually, making it invaluable for confirming vulnerabilities while reducing false positives.

Finally, this room reinforced that a solid understanding of HTTP communication is fundamental to web application security, as every vulnerability ultimately originates from how the server interprets incoming requests.

---

# Challenges Encountered

### Understanding Raw HTTP Messages

Initially, the large number of HTTP headers made the requests appear overwhelming. Distinguishing between browser-generated headers and those that directly influenced the application required careful observation and experimentation.

---

### Analysing Large Responses

The returned HTML responses contained thousands of lines of code, making it difficult to quickly identify meaningful output. Switching between the **Pretty**, **Raw**, and **Render** views helped improve navigation and response analysis.

---

### Maintaining Request Accuracy

Manual request manipulation requires precision. Small syntax errors, accidental modifications to headers, or incorrect payload placement can cause requests to fail or generate misleading results. This emphasised the importance of making incremental changes and validating each response carefully.

---

# Screenshots

## SQL Injection Request

**Description**

Modified the intercepted HTTP request using Burp Repeater by injecting a UNION-based SQL Injection payload before sending it to the server.

**Screenshot**

<img width="683" height="657" alt="image" src="https://github.com/user-attachments/assets/f8065a38-4f42-4697-91c8-c64fcf067512" />

---

## Server Response

**Description**

The modified request successfully returned additional database information, revealing the TryHackMe flag inside the application's HTML response and confirming successful SQL Injection.

**Screenshot**


<img width="679" height="503" alt="image" src="https://github.com/user-attachments/assets/ded57e3c-faae-499c-9faf-337ceb212eef" />

---

# Key Takeaways

- Burp Repeater is one of the most important tools for manual web application testing.
- Understanding raw HTTP communication is fundamental for identifying web vulnerabilities.
- Comparing server responses after small request modifications is an effective vulnerability discovery technique.
- Manual testing provides greater insight into application behaviour than relying solely on automated scanners.
- SQL Injection vulnerabilities can often be confirmed through carefully crafted HTTP requests and detailed response analysis.
- Burp Repeater is widely used during professional penetration tests for vulnerability verification, debugging, API testing, and security research.

---

# Overall Reflection

The Burp Suite: Repeater room strengthened my understanding of manual web application security testing and demonstrated the importance of analysing both HTTP requests and server responses in detail. Although the lab focused on a SQL Injection example, the techniques learned are applicable to many other vulnerabilities, including authentication bypass, access control testing, command injection, and API security assessments.

Overall, this room provided a strong foundation for future Burp Suite modules and reinforced the importance of methodical, manual testing when assessing the security of modern web applications.
