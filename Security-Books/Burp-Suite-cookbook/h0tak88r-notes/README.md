
# Download Book
https://www.pdfdrive.com/burp-suite-essentialspdf-e49612640.html
### Burp version to 1.7.36 or higher.
### Blugins 
- Software Vulnerability  
- Retire.js 
- active scan ++
- JSON Beautifier and JSON Web Tokens:
- Java Serial Killer

# What this book covers
Chapter 1, Getting Started with Burp Suite, provides setup instructions necessary
to proceed through the material of the book.

Chapter 2, Getting to Know the Burp Suite of Tools, begins with establishing the
Target scope and provides overviews to the most commonly used tools within
Burp Suite.

Chapter 3, Configuring, Spidering, Scanning, and Reporting with Burp, helps
testers to calibrate Burp settings to be less abusive towards the target application.

Chapter 4, Assessing Authentication Schemes, covers the basics of
Authentication, including an explanation that this is the act of verifying a person
or object claim is true.

Chapter 5, Assessing Authorization Checks, helps you understand the basics of
Authorization, including an explanation that this how an application uses roles to
determine user functions.

Chapter 6, Assessing Session Management Mechanisms, dives into the basics of
Session Management, including an explanation that this how an application
keeps track of user activity on a website.

Chapter 7, Assessing Business Logic, covers the basics of Business Logic
Testing, including an explanation of some of the more common tests performed
in this area.

Chapter 8, Evaluating Input Validation Checks, delves into the basics of Data
Validation Testing, including an explanation of some of the more common tests
performed in this area.

Chapter 9, Attacking the Client, helps you understand how Client-Side testing is
concerned with the execution of code on the client, typically natively within a
web browser or browser plugin. Learn how to use Burp to test the execution of
code on the client-side to determine the presence of Cross-site Scripting (XSS).

Chapter 10, Working with Burp Macros and Extensions, teaches you how Burp
macros enable penetration testers to automate events such as logins or response
parameter reads to overcome potential error situations. We will also learn about
Extensions as an additional functionality to Burp.

Chapter 11, Implementing Advanced Topic Attacks, provides a brief explanation
of XXE as a vulnerability class targeting applications which parse XML and
SSRF as a vulnerability class allowing an attacker to force applications to make
unauthorized requests on the attacker’s behalf.

# CH1: Getting Started with Burp Suite

- Introduction

### Downloading Burp (Community, Professional)

The first step in learning the techniques contained within this book is to
download the Burp suite. The download page is available here
([https://portswigger.net/burp/](https://portswigger.net/burp/)). You will need to decide which edition of the Burp
suite you would like to download from the following:

- Professional
- Community
- Enterprise (not covered)

### Starting Burp at a command line or
as an executable

1. Start Burp in Windows, after running the installer from the `downloaded.exe` file, by double-clicking the icon on desktop or select it from the programs listing.
2. Start Burp at the command line (minimal) with the plain JAR file (Java
must be installed first): 

```jsx
C:\Burp jar files>java -jar burpsuite_pro_1.7.33.jar
```

### Listening for HTTP traffic, using Burp

1. In the General tab, scroll down to the Network Proxy section and then click
Settings.
2. In the Connection Settings, select Manual proxy configuration and type in
the IP address of 127.0.0.1 with port 8080. Select the Use this proxy server
for all protocols checkbox:
3. Make sure the No proxy for the textbox is blank, as shown in the following
screenshot, and then click OK:
4. Or Use `Foxyproxy` extension
