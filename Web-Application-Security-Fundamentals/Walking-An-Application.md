# TryHackMe Write-Up: Walking an Application

## Room Overview

This room introduced the process of manually exploring a web application using browser-based tools. Rather than actively attacking the application, the focus was on reconnaissance and information gathering techniques. Throughout the room, I explored page source code, hidden links, exposed directories, web framework identification, and browser Developer Tools to uncover information that was not immediately visible to users.

---

## Objectives

* Explore a web application manually
* Analyse HTML source code
* Discover hidden pages and links
* Identify exposed directory listings
* Fingerprint web application frameworks
* Use browser Developer Tools for investigation
* Understand how client-side information can be exposed

---

## Key Concepts

### Viewing Page Source

Web pages are built using HTML, CSS, and JavaScript. Browsers render this code into the visual page users interact with.

By viewing the page source, it is often possible to discover:

* Hidden comments
* Developer notes
* Internal links
* Metadata
* Application structure

Information found in source code may not be visible on the webpage itself.

---

### Hidden Links

Developers sometimes leave links within source code that are not directly accessible through the user interface.

These hidden links can reveal:

* Administrative pages
* Testing environments
* Backup files
* Sensitive application functionality

Reviewing source code is therefore an important reconnaissance technique.

---

### Directory Listings

A directory listing occurs when a web server allows users to view the contents of a directory.

Potential risks include:

* Exposure of backup files
* Source code disclosure
* Sensitive documents
* Configuration files

Attackers often look for directory listings during initial enumeration.

---

### Framework Fingerprinting

Many websites use frameworks to simplify development.

Examples include:

* Bootstrap
* React
* Angular
* Vue.js
* Django
* Flask

Identifying a framework can help security professionals understand:

* Application architecture
* Potential attack surfaces
* Known vulnerabilities associated with specific versions

---

## Developer Tools

Modern browsers provide Developer Tools that allow detailed analysis of web applications.

### Inspector

The Inspector tab allows examination of a webpage's Document Object Model (DOM).

Uses include:

* Viewing hidden elements
* Modifying page content locally
* Understanding page structure
* Analysing HTML and CSS

---

### Debugger

The Debugger (Sources tab) allows inspection of JavaScript files executed by the browser.

Uses include:

* Reviewing client-side code
* Understanding application logic
* Identifying hidden information
* Troubleshooting scripts

---

### Network

The Network tab records communication between the browser and web server.

Uses include:

* Viewing HTTP requests
* Analysing server responses
* Identifying API endpoints
* Inspecting transmitted data

This is one of the most valuable tools during web application testing.

---

### Storage

The Storage tab provides access to data stored within the browser.

Examples include:

* Cookies
* Local Storage
* Session Storage

Developers sometimes store information client-side that may be accessible through these storage mechanisms.

---

## Skills Learned

* Viewing and analysing HTML source code
* Identifying hidden links and resources
* Discovering exposed directory listings
* Basic web application fingerprinting
* Using browser Developer Tools effectively
* Inspecting DOM elements with Inspector
* Analysing JavaScript through the Debugger
* Monitoring HTTP traffic using the Network tab
* Examining browser storage mechanisms
* Understanding client-side information disclosure risks

---

## What I Learned

This room demonstrated how much information can be gathered from a web application without performing any attacks. Through careful observation and enumeration, I discovered hidden resources, identified application technologies, and located information stored within the browser.

I also gained practical experience using browser Developer Tools, which are essential for web application testing and security assessments. The room reinforced the importance of reconnaissance and showed how seemingly harmless information disclosures can reveal valuable insights about an application's structure and functionality.

---

# Screenshots

## Screenshot 1 - HTML Source Code Flag

<img width="584" height="83" alt="image" src="https://github.com/user-attachments/assets/b77d409a-fd2e-4186-8816-8eee9354a519" />
<img width="609" height="103" alt="image" src="https://github.com/user-attachments/assets/bed00468-a366-4e4f-8747-4011333eecc7" />


Description:

Viewed the page source and identified information hidden within the HTML code that was not visible on the webpage itself.

---

## Screenshot 2 - Hidden Link Discovery

<img width="723" height="179" alt="image" src="https://github.com/user-attachments/assets/0085b21e-c8df-4fcb-a952-9bdb9fb7a93c" />

<img width="724" height="166" alt="image" src="https://github.com/user-attachments/assets/7d2eb292-c177-4341-8eeb-d77fccfb7d39" />


Description:

Discovered a hidden link embedded within the source code and followed it to access additional content.

---

## Screenshot 3 - Directory Listing

<img width="632" height="88" alt="image" src="https://github.com/user-attachments/assets/1dbdea13-cacf-4920-8326-789526e9501f" />

<img width="629" height="277" alt="image" src="https://github.com/user-attachments/assets/d592dc2f-8777-49c1-9eff-1a47c40f68ee" />


Description:

Identified an exposed directory listing which revealed accessible files and resources hosted on the server.

---

## Screenshot 4 - Framework Identification

<img width="651" height="190" alt="image" src="https://github.com/user-attachments/assets/d79686eb-31ba-424b-9879-6bdd37c646c4" />

<img width="418" height="151" alt="image" src="https://github.com/user-attachments/assets/a1ff63a9-6bea-4aed-8a40-303e6e67f1f2" />


Description:

Identified the framework used by the application through publicly available information exposed within the website.

---

## Screenshot 5 - Developer Tools: Inspector

<img width="727" height="595" alt="image" src="https://github.com/user-attachments/assets/5db680ce-c6b3-4e42-935a-3b0ab62cd1de" />


Description:

Used the Inspector tool to analyse the page structure and locate hidden elements within the DOM.

---

## Screenshot 6 - Developer Tools: Debugger

<img width="723" height="622" alt="image" src="https://github.com/user-attachments/assets/66780eb5-279c-4963-8e80-d9ed40294a63" />


Description:

Examined JavaScript source files using the Debugger to understand client-side functionality and locate hidden information.

---

## Screenshot 7 - Developer Tools: Network

<img width="733" height="539" alt="image" src="https://github.com/user-attachments/assets/40673405-3681-4032-a855-d3f4c8fcf921" />


Description:

Captured and analysed HTTP requests and responses to observe communication between the browser and web server.

---


## What I Learned

- Information hidden within HTML source code can reveal sensitive details that are not visible through the normal user interface.
- Hidden links and pages can often be discovered through manual inspection of a website's source code.
- Exposed directory listings may unintentionally reveal files, backups, and other resources hosted on a server.
- Framework fingerprinting can provide valuable information about the technologies used by a web application.
- Browser Developer Tools are powerful reconnaissance tools that allow analysts to inspect page structure, JavaScript code, network traffic, and stored data.
- The Inspector tab can be used to analyse and modify page elements locally for testing purposes.
- The Debugger tab helps investigate client-side JavaScript and understand how an application functions.
- The Network tab provides visibility into HTTP requests and responses exchanged between the browser and server.
- Browser storage mechanisms such as cookies, local storage, and session storage may contain useful information about application behaviour.
- Effective web application security testing begins with thorough reconnaissance and information gathering before attempting exploitation.
---

## Challenges Faced

One of the challenges during this room was learning where information is commonly stored within a web application. Since the tasks required manual exploration rather than terminal commands, it was necessary to carefully inspect different areas of the website and browser tools to locate hidden information.

Another challenge was becoming familiar with the various tabs within Developer Tools and understanding the purpose of each one.

---

## Reflection

This room highlighted the importance of reconnaissance in web application security. Rather than focusing on exploitation, the exercises demonstrated how valuable information can be gathered through observation alone.

The experience improved my confidence in using browser-based investigation tools and reinforced the idea that understanding an application's structure is often the first step in identifying security weaknesses. These skills provide a strong foundation for future web application testing and penetration testing activities.
