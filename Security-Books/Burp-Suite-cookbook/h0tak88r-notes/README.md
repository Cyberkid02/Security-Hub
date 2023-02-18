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

# CH2:Getting to Know the Burp Suite of Tools

In this chapter, we will cover the following recipes:
Setting the Target Site Map

- Understanding Message Editor
- Repeating with Repeater
- Decoding with Decoder
- Intruding with Intruder

# CH3:Configuring, Spidering, Scanning, and Reporting

In this chapter, we will cover the following recipes:
Establishing trust over HTTPS

- Setting project options
- Setting user options
- Spidering with Spider
- Scanning with Scanner
- Reporting issues
