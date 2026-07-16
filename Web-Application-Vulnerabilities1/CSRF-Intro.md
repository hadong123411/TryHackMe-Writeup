# SQL Injection Introduction

**Platform:** TryHackMe  
**Category:** Web Application Security  
**Difficulty:** Easy  
**Estimated Time:** 60 Minutes  
**Status:**  Completed

---

# Overview

The **SQL Injection Introduction** room provides a comprehensive introduction to one of the most well-known and impactful web application vulnerabilities: **SQL Injection (SQLi)**. SQL Injection occurs when user-supplied input is incorrectly incorporated into SQL queries without proper validation or sanitisation, allowing attackers to manipulate database commands.

This room explains the fundamentals of SQL databases, how SQL queries operate, why SQL Injection occurs, and how attackers exploit vulnerable applications to retrieve sensitive information, bypass authentication, or execute unintended database commands.

In addition to learning offensive techniques, the room also introduces defensive strategies such as parameterised queries, prepared statements, and input validation to mitigate SQL Injection attacks.

---

# Objectives

- Understand the basics of SQL databases.
- Learn common SQL statements and syntax.
- Understand how SQL Injection vulnerabilities occur.
- Explore different types of SQL Injection attacks.
- Learn authentication bypass techniques.
- Understand the difference between In-Band, Blind and Out-of-Band SQL Injection.
- Learn common prevention and remediation techniques.
- Complete practical SQL Injection exercises.

---

# Tools Used

- Web Browser
- TryHackMe AttackBox
- SQL Query Examples
- Vulnerable Web Application
- Linux Terminal
- HTML Editor (Nano)

---

# Module Notes

## What is SQL?

**SQL (Structured Query Language)** is the standard language used to communicate with relational database management systems (RDBMS).

Applications such as online stores, banking systems, social media platforms and healthcare systems all rely on SQL databases to store and retrieve information.

Common database systems include:

- MySQL
- PostgreSQL
- Microsoft SQL Server
- Oracle Database
- SQLite

Applications interact with these databases by sending SQL queries whenever users:

- Log in
- Register accounts
- Search for products
- Update personal information
- Submit forms
- Retrieve records

---

## Database Structure

Relational databases organise information into **tables**.

Each table consists of:

- Rows (records)
- Columns (fields)

Example:

| ID | Username | Email |
|----|----------|----------------------|
| 1 | Alice | alice@email.com |
| 2 | Bob | bob@email.com |

Each row represents an individual record, while each column stores a specific attribute.

Multiple tables can be linked together using **primary keys** and **foreign keys**, allowing complex relationships between data.

---

## Common SQL Commands

The room introduced several commonly used SQL statements.

### SELECT

Retrieves information from a database.

```sql
SELECT username,email
FROM users;
```

---

### WHERE

Filters returned records.

```sql
SELECT *
FROM users
WHERE username='admin';
```

---

### INSERT

Adds new records.

```sql
INSERT INTO users(username,password)
VALUES('Alice','Password123');
```

---

### UPDATE

Modifies existing records.

```sql
UPDATE users
SET email='new@email.com'
WHERE id=5;
```

---

### DELETE

Removes records.

```sql
DELETE FROM users
WHERE id=5;
```

Understanding these statements is essential because SQL Injection works by manipulating legitimate SQL queries executed by the database.

---

## What is SQL Injection?

SQL Injection occurs when an application directly inserts user input into an SQL statement without validating or sanitising it.

For example, an application may construct a query like:

```sql
SELECT *
FROM users
WHERE username='admin'
AND password='password';
```

If user input is not properly handled, an attacker can inject additional SQL commands into the query.

Example:

```text
' OR 1=1--
```

The resulting SQL query becomes:

```sql
SELECT *
FROM users
WHERE username=''
OR 1=1--;
```

Because **1=1** always evaluates to **TRUE**, the database returns all matching records, potentially bypassing authentication entirely.

---

## Why SQL Injection Happens

SQL Injection vulnerabilities generally occur because developers concatenate user input directly into SQL queries.

Unsafe example:

```php
$query = "SELECT * FROM users WHERE username = '$username'";
```

Since the input is treated as executable SQL code rather than plain text, attackers can manipulate the entire query.

Modern applications should instead use:

- Prepared Statements
- Parameterised Queries
- ORM Frameworks
- Input Validation

---

## Types of SQL Injection

The room introduced three major categories of SQL Injection.

| Type | Description |
|-------|-------------|
| In-Band SQL Injection | Data is retrieved through the same communication channel. |
| Blind SQL Injection | No direct output is shown; attackers infer information from application behaviour. |
| Out-of-Band SQL Injection | Data is exfiltrated through a separate communication channel. |

Each technique targets the same underlying vulnerability but extracts information differently depending on how the application responds.

---

# In-Band SQL Injection

In-Band SQL Injection is the most common form of SQL Injection.

Both the attack payload and the retrieved information travel through the same communication channel.

Two common techniques include:

- Error-Based SQL Injection
- UNION-Based SQL Injection

---

## Error-Based SQL Injection

Some applications display database error messages whenever malformed SQL statements are executed.

These error messages may reveal:

- Database type
- Table names
- Column names
- SQL syntax
- Server configuration

Example:

```sql
'
```

A single quotation mark may generate a detailed SQL syntax error, allowing attackers to understand how the backend database processes queries.

Properly configured production systems should never expose raw database errors.

---

## UNION-Based SQL Injection

UNION allows attackers to combine the results of multiple SQL queries into a single response.

Example:

```sql
UNION SELECT username,password
FROM users;
```

If the application displays the results, attackers may retrieve sensitive database information such as usernames, password hashes or administrator accounts.

UNION-based attacks require:

- Matching column counts
- Compatible data types
- Visible application responses

This makes UNION attacks one of the most powerful forms of SQL Injection when successful.

---

# Authentication Bypass

One of the most common SQL Injection attacks targets login forms.

Instead of providing a legitimate username and password, attackers inject SQL operators that force the authentication query to always evaluate as **TRUE**.

Example payload:

```text
' OR '1'='1'--
```

Example query:

```sql
SELECT *
FROM users
WHERE username=''
OR '1'='1';
```

Since the condition is always true, the application may log the attacker in without verifying valid credentials.

Authentication bypass demonstrates why relying solely on SQL queries for access control is extremely dangerous when user input is not sanitised.

---

# Practical Exercise Summary

During the practical exercises, I explored how SQL Injection vulnerabilities could be identified and exploited within intentionally vulnerable web applications.

The room demonstrated how modifying SQL queries could bypass authentication, retrieve hidden information and manipulate backend database operations.

I also completed practical challenges involving HTML modification and simulated web application exploitation to better understand how vulnerable applications can be abused when secure coding practices are not followed.

---

# Screenshot Placeholders

## Screenshot 1 — Room Completion

**Description**

Completed the SQL Injection Introduction room and successfully finished all learning tasks.

<img width="632" height="294" alt="image" src="https://github.com/user-attachments/assets/b4adf759-c442-42f3-9a38-d1f6867814e8" />


---

## Screenshot 2 — Practical Challenge Setup

**Description**

Prepared the vulnerable web application's HTML files and navigated to the application directory before modifying the attack page.

<img width="865" height="328" alt="image" src="https://github.com/user-attachments/assets/37fc4f21-13c7-4754-8c59-0426f218faa6" />


---

## Screenshot 3 — HTML Payload

**Description**

Modified the HTML page to automatically submit a malicious request targeting the vulnerable application.

<img width="1335" height="571" alt="image" src="https://github.com/user-attachments/assets/1fb27b78-3b81-4eda-923a-9d454455e46c" />


# Blind SQL Injection

Unlike In-Band SQL Injection, **Blind SQL Injection** occurs when the application does not directly display database information or SQL error messages.

Although attackers cannot immediately view the contents of the database, they can still determine information by carefully observing how the application responds to specially crafted SQL queries.

Blind SQL Injection relies on indirect evidence such as:

- Different webpage responses
- Authentication success or failure
- Changes in page content
- Response time delays
- Server behaviour

For this reason, Blind SQL Injection is generally slower than In-Band SQL Injection but remains highly effective against poorly secured applications.

---

## Authentication Bypass

One of the simplest forms of Blind SQL Injection targets login pages.

Instead of retrieving database information, the attacker attempts to manipulate the authentication query.

Example payload:

```text
' OR '1'='1'--
```

Original query:

```sql
SELECT *
FROM users
WHERE username='admin'
AND password='password';
```

Injected query:

```sql
SELECT *
FROM users
WHERE username=''
OR '1'='1'--;
```

Since the condition **1=1** always evaluates to TRUE, the application may authenticate the attacker without validating legitimate credentials.

Authentication bypass demonstrates why user input should never be concatenated directly into SQL statements.

---

## Boolean-Based Blind SQL Injection

Boolean-Based SQL Injection determines whether statements are **TRUE** or **FALSE** based on changes in application behaviour.

Example:

```sql
' AND 1=1--
```

If the webpage behaves normally, the attacker knows the condition evaluated as TRUE.

Changing the query:

```sql
' AND 1=2--
```

If the webpage changes, returns fewer results, or behaves differently, the attacker confirms that FALSE conditions produce a different response.

By repeatedly asking TRUE or FALSE questions, attackers can slowly reconstruct sensitive database information such as:

- Database names
- Table names
- Column names
- Usernames
- Password hashes

Although this method is slower than UNION-based attacks, it remains effective when error messages are hidden.

---

## Time-Based Blind SQL Injection

Some applications always return identical pages regardless of whether a condition is TRUE or FALSE.

In these situations, attackers use **time delays** to determine whether injected SQL statements execute successfully.

Example:

```sql
' IF(1=1,SLEEP(5),0)--
```

If the server pauses for five seconds before responding, the attacker knows the injected condition evaluated as TRUE.

Changing the condition:

```sql
' IF(1=2,SLEEP(5),0)--
```

If the page immediately responds, the attacker knows the condition evaluated as FALSE.

By measuring response times, attackers can gradually extract database information one character at a time.

Although extremely slow, Time-Based SQL Injection works even when applications reveal almost no information.

---

# Out-of-Band SQL Injection

Out-of-Band (OOB) SQL Injection retrieves information through a completely separate communication channel.

Instead of displaying results within the web application, the database sends information externally.

Common techniques include:

- DNS requests
- HTTP callbacks
- SMB connections
- External web requests

For example, an attacker may force the database server to send a DNS lookup containing sensitive information to a server under their control.

This technique is generally used when:

- Error messages are disabled.
- Responses never change.
- Time-based attacks are impractical.
- Direct data extraction is impossible.

Although less common than other SQL Injection methods, Out-of-Band SQL Injection can be extremely powerful in complex enterprise environments.

---

# Prevention and Remediation

The room concluded by discussing secure development practices that prevent SQL Injection vulnerabilities.

---

## Prepared Statements

Prepared statements separate SQL logic from user input.

Instead of treating user input as executable SQL code, the database treats it as plain data.

Unsafe query:

```php
$query = "SELECT * FROM users WHERE username='$username'";
```

Secure query:

```php
$stmt = $pdo->prepare(
"SELECT * FROM users WHERE username=?"
);
```

Prepared statements remain the industry standard defence against SQL Injection.

---

## Parameterised Queries

Parameterised queries bind user input separately from the SQL statement.

Advantages include:

- Prevent SQL Injection
- Improve code readability
- Simplify maintenance
- Reduce developer mistakes

Nearly every modern programming language provides built-in support for parameterised queries.

---

## Input Validation

Applications should validate user input before processing it.

Validation techniques include:

- Allow lists
- Data type checking
- Length restrictions
- Character filtering
- Input sanitisation

Although validation improves security, it should never replace prepared statements.

---

## Principle of Least Privilege

Database accounts should only possess permissions required for their intended function.

For example:

- Read-only accounts should never delete records.
- Web applications should not connect using administrator accounts.
- Administrative functions should use separate credentials.

Limiting database permissions reduces the impact of successful SQL Injection attacks.

---

## Error Handling

Applications should never expose raw SQL error messages to users.

Instead, production systems should:

- Display generic error pages.
- Log detailed errors internally.
- Prevent disclosure of database information.

Proper error handling removes valuable reconnaissance information from attackers.

---

# Practical Exercise Summary

During the practical exercises, I completed several scenarios demonstrating SQL Injection concepts within intentionally vulnerable web applications.

The lab involved modifying HTML files, preparing malicious requests, and exploiting vulnerable functionality to manipulate application behaviour.

One challenge involved automatically updating a victim's email address through a crafted HTML form, demonstrating how insecure application logic can be abused when proper validation and security controls are absent.

Another challenge required manipulating application functionality to change user privileges, reinforcing how insecure input handling can directly affect access control mechanisms.

These practical exercises demonstrated how relatively small implementation mistakes can have significant security consequences.

---

# What I Learned

- How SQL databases execute application queries.
- Why SQL Injection remains one of the most dangerous web vulnerabilities.
- The differences between In-Band, Blind and Out-of-Band SQL Injection.
- How authentication bypass attacks manipulate SQL logic.
- How Boolean-Based SQL Injection extracts information through TRUE/FALSE responses.
- How Time-Based SQL Injection uses server response delays to infer database information.
- Why prepared statements are the most effective defence against SQL Injection.
- How least privilege limits the impact of successful database compromise.
- Why hiding SQL errors improves application security.
- The importance of secure coding practices during web application development.

---

# What I Found Interesting

The most interesting concept introduced in this room was **Blind SQL Injection**.

Initially, I assumed attackers always needed visible database output to exploit SQL Injection. Learning that attackers can reconstruct entire databases simply by observing webpage behaviour or measuring response times demonstrated how dangerous SQL Injection can be even when applications reveal almost no information.

I also found Out-of-Band SQL Injection particularly interesting because it illustrated that attackers are not limited to communicating through the vulnerable website itself. Using alternative communication channels such as DNS or HTTP requests demonstrated the creativity involved in advanced penetration testing.

Finally, I found the prevention section especially valuable because it reinforced that SQL Injection vulnerabilities are almost entirely preventable through secure development practices such as prepared statements and parameterised queries. This highlighted how many critical security vulnerabilities originate from insecure coding decisions rather than weaknesses in SQL itself.

---

# Screenshot Placeholders

## Screenshot 4 — Email Update Payload

**Description**

Displayed the crafted HTML form used to automatically submit a malicious request that changed the target email address.

<img width="1335" height="571" alt="image" src="https://github.com/user-attachments/assets/b58b348a-7250-48e9-8881-dd1661d21206" />


---

## Screenshot 5 — Email Update Challenge

**Description**

Successfully completed the practical challenge by updating the vulnerable application's email address and obtaining the room flag.

<img width="1129" height="862" alt="image" src="https://github.com/user-attachments/assets/270dd384-dffb-49d6-946f-dfacec175ff6" />


---

## Screenshot 6 — Role Manipulation Challenge

**Description**

Accessed the vulnerable StaffHub application demonstrating insecure role management functionality before exploiting the challenge.

<img width="1030" height="699" alt="image" src="https://github.com/user-attachments/assets/fd9d21e7-feaa-4fc3-96bb-d6ee09288553" />


---

## Screenshot 7 — Final Practical Challenge

**Description**

Successfully manipulated the vulnerable application's role assignment and email configuration, completing the final SQL Injection practical challenge and obtaining both room flags.

<img width="1141" height="997" alt="image" src="https://github.com/user-attachments/assets/25052017-1ca4-402d-8c3a-3acdee77c466" />


# Challenges Encountered

### Understanding SQL Query Logic

Initially, understanding how user input becomes part of an SQL query required careful analysis. Although the syntax appeared straightforward, visualising how the database interprets injected characters such as quotation marks (`'`), comments (`--`), and logical operators (`OR`, `AND`) took some experimentation.

Working through practical examples helped reinforce how even a single injected character can completely alter the behaviour of a database query.

---

### Distinguishing SQL Injection Techniques

At first, the differences between **In-Band**, **Blind**, and **Out-of-Band** SQL Injection seemed subtle because they all exploit the same underlying vulnerability.

After studying each attack method, I developed a clearer understanding that the primary difference lies in **how attackers retrieve information** rather than how the vulnerability itself occurs.

- In-Band SQL Injection returns data directly through the application's normal responses.
- Blind SQL Injection infers information through application behaviour or response timing.
- Out-of-Band SQL Injection retrieves information using alternative communication channels such as DNS or HTTP requests.

Understanding these distinctions helped clarify when each technique would be applicable during a penetration test.

---

### Understanding Boolean Logic

One of the more challenging concepts involved Boolean-Based SQL Injection.

Although the injected queries themselves were relatively simple, understanding how attackers gradually reconstruct database information using only TRUE and FALSE responses required careful thought.

Learning this technique demonstrated that attackers do not always need direct access to database output to successfully compromise sensitive information.

---

### Appreciating Defensive Techniques

Initially, SQL Injection appeared to be a highly technical vulnerability requiring complex exploitation techniques.

However, the prevention section highlighted that many SQL Injection vulnerabilities originate from insecure coding practices rather than weaknesses in SQL itself.

Understanding how prepared statements, parameterised queries, and least-privilege database accounts prevent SQL Injection reinforced the importance of secure software development throughout the application lifecycle.

---

# Key Takeaways

- SQL Injection remains one of the most critical web application vulnerabilities and is consistently recognised within the **OWASP Top 10**.
- SQL Injection occurs when user input is incorporated into SQL queries without proper validation or parameterisation.
- Attackers can exploit vulnerable applications to bypass authentication, retrieve sensitive information, modify database records, or execute unintended SQL commands.
- In-Band SQL Injection is the most common form because database output is returned directly to the attacker.
- Blind SQL Injection demonstrates that attackers can still extract sensitive information even when no database output is displayed.
- Out-of-Band SQL Injection uses alternative communication channels when traditional extraction methods are unavailable.
- Prepared statements and parameterised queries are considered the most effective defence against SQL Injection.
- Secure error handling and least-privilege database accounts significantly reduce the impact of successful attacks.
- Understanding how databases process user input is essential for both penetration testers and defensive security professionals.
- Secure coding practices remain one of the strongest defences against modern web application attacks.

---

# Overall Reflection

This room provided one of the most valuable introductions to web application security that I have completed so far. Rather than focusing solely on exploitation techniques, it explained **why SQL Injection occurs**, how attackers think when identifying vulnerable applications, and how developers can prevent these vulnerabilities through secure coding practices.

The practical exercises reinforced that SQL Injection is not simply about injecting random payloads into input fields. Successful exploitation requires understanding SQL syntax, database behaviour, application logic, and how different responses reveal information about backend systems.

The room also demonstrated that SQL Injection extends beyond simple authentication bypasses. Techniques such as Boolean-Based, Time-Based, and Out-of-Band SQL Injection highlighted how attackers can continue extracting sensitive information even when applications appear to provide little or no feedback.

From a defensive perspective, this room reinforced the importance of secure software development. Concepts such as prepared statements, parameterised queries, input validation, secure error handling, and least-privilege database permissions demonstrated that preventing SQL Injection is primarily a matter of implementing secure coding practices rather than relying on reactive security controls.

Overall, this room significantly strengthened my understanding of one of the most common and impactful web application vulnerabilities. The knowledge gained will provide a strong foundation for more advanced topics such as authentication testing, API security, command injection, cross-site scripting (XSS), and broader OWASP Top 10 vulnerability assessments.

---

# References

- OWASP Top 10 – Injection
- SQL (Structured Query Language) Fundamentals
- TryHackMe: SQL Injection Introduction
- Burp Suite Documentation
- Prepared Statements and Parameterised Queries Best Practices

---

# Skills Developed

## Offensive Security

- SQL Injection Testing
- Authentication Bypass
- UNION-Based SQL Injection
- Blind SQL Injection
- Boolean Logic Testing
- Time-Based Injection
- Out-of-Band SQL Injection

---

## Defensive Security

- Secure Coding Awareness
- Prepared Statements
- Parameterised Queries
- Input Validation
- Error Handling
- Principle of Least Privilege

---

## Web Application Security

- SQL Query Analysis
- Database Fundamentals
- HTTP Request Analysis
- Web Vulnerability Assessment
- Secure Development Concepts

---

# Reflection

This room represents an important milestone in developing practical web application security skills. SQL Injection is one of the most widely recognised vulnerabilities within offensive security and continues to appear in real-world applications despite being well understood.

Completing this room improved not only my ability to recognise vulnerable SQL queries but also my understanding of how attackers approach database exploitation and how developers can design applications that resist these attacks.

Combined with my previous Burp Suite documentation, this room further strengthened my knowledge of manual web application testing and reinforced the importance of understanding both offensive techniques and defensive mitigation strategies when assessing modern web applications.
