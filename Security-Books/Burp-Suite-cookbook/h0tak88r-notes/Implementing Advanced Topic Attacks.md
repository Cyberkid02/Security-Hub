# CH11:Implementing Advanced Topic Attacks

In this chapter, we will cover the following recipes:

- **Performing XML External Entity (XXE) attacks**
    
    ### How to do it...
    
    1. Navigate to the XML External Entity Injection page, that is, through Others | XML External Entity Injection | XML Validator:
    2. While on the XML Validator page, perform the example XML that is provided on the page. Click on the Validate XML button:
    3. Switch to **Burp Proxy| HTTP history** tab and look for the request you just submitted to validate the XML. Right-click and send the request to the **repeater**:
    4. Note the value provided in the **xml parameter**:
    5. Use Burp Proxy Interceptor to replace this XML parameter value with the following payload. This new payload will make a request to a file on the operating system that should be restricted from view, namely, the `/etc/passwd` file:
        
        ```jsx
        <?xml version="1.0"?>
        <!DOCTYPE change-log[
        <!ENTITY systemEntity SYSTEM
        "../../../../etc/passwd">
        ]>
        <change-log>
        <text>&systemEntity;</text>
        </change-log>
        Since there are odd characters and spaces in the new XML message, let's
        type this payload into the Decoder section and URL-encode it before we
        paste it into the xml parameter.
        ```
        
    6. Switch to the Decoder section, type or paste the new payload into the text area. Click the Encode as… button and select the URL option from the drop-down listing. Then, copy the URL-encoded payload using Ctrl + C. Make sure you copy all of the payload by scrolling to the right:
        
        ![image](https://user-images.githubusercontent.com/108616378/219864288-f646b851-cfbd-428b-bb00-ff68d2029355.png)
        
    7. Switch to the Burp Proxy Intercept tab. Turn the interceptor on with
    the Intercept is on button.
    8. Return to the Firefox browser and reload the page. As the request is paused,
    replace the current value of the xml parameter with the new URL-encoded
    payload:
        
        ![image](https://user-images.githubusercontent.com/108616378/219864315-4908c8c7-9d8b-46cc-8292-b830e3b894a8.png)
        
    9. Click the Forward button. Turn interceptor off by toggling the button to
    Intercept is off.
    10. Note that the returned XML now shows the contents of the /etc/passwd
    file! The XML parser granted us access to the /etc/passwd file on the
    operating system:
- **Working with JSON Web Token (JWT)**
    1. Switch to `Burp BApp Store` and install two plugins→ `JSON Beautifier` and `JSON Web Tokens`:
    2. In the Firefox browser, go to your `OneLogin` page. The URL will be specific to the developer account you created. Log in to the account using the credentials you established when you set up the account before beginning this recipe:
    3. Switch to the **Burp Proxy | HTTP history tab**. Find the **POST** request with the **URL /access/auth**. Right-click and click the Send to **Repeater option**.
    4. Your host value will be specific to the `OneLogin` account you set up:
    5. Switch to the Repeater tab and notice that you have two additional tabs relating to the two extensions you installed:
        
        ![image](https://user-images.githubusercontent.com/108616378/219864380-ba8e4410-315c-4490-9201-7f947e42ae19.png)
        
    6. Click the JSON Beautifier tab to view the JSON structure in a more readable manner:
        
        ![image](https://user-images.githubusercontent.com/108616378/219864421-28f85538-f860-4947-963f-11f0dea4a280.png)
        
    7. Click the JSON Web Tokens tab to reveal a debugger very similar to the one available at [https://jwt.io](https://jwt.io/). This plugin allows you to read the claims content and manipulate the encryption algorithm for various brute-force tests. For example, in the following screenshot, notice how you can change the algorithm to `nOnE` in order to attempt to create a new JWT token to place into the request:
        
        ![image](https://user-images.githubusercontent.com/108616378/219864452-43ff11fe-6cba-4de2-b73f-1d21c00925b7.png)
        
    
    ### How it works...
    
    Two extensions, JSON Beautifier and JSON Web Tokens, help testers to work
    with JWT tokens in an easier way by providing debugger tools conveniently
    available with the Burp UI
    
- **Using Burp Collaborator to determine Server-Side Request Forgery(SSRF)**
    
    ### How to do it...
    
    1. Switch to the Burp **`Project options | Misc tab`**. Note the `Burp Collaborator` **Server section**. You have options available for using a private Burp Collaborator server, which you would set up, or you may use the publicly
    **internet-accessible** one made available by **`PortSwigger`**. For this recipe, we will use the public one:
    2. Check the box labeled Poll over unencrypted HTTP and click the Run health check… button:
        
        []()
        
    3. A pop-up box appears to test various protocols to see whether they will connect to the public Burp Collaborator server available on the internet.
    4. Check the messages for each protocol to see which are successful. Click the Close button when you are done:
    5. From the top-level menu, select Burp | Burp Collaborator client:
    6. A pop-up box appears. In the section labeled Generate Collaborator payloads, change the 1 to 10:
    7. Click the Copy to clipboard button. Leave all other defaults as they are. Do
    not close the Collaborator client window. If you close the window, you will
    lose the client session:
    8. Return to the Firefox browser and navigate to OWASP 2013 | A1 – Injection (Other) | HTML Injection (HTMLi) | DNS Lookup:
    9. On the DNS Lookup page, type an IP address and click the Lookup DNS button:
    10. Switch to the Burp Proxy | HTTP history tab and find the request you just created on the DNS Lookup page. Right-click and select the Send to Intruder option:
    11. Switch to the Burp Intruder | Positions tab. Clear all suggested payload markers and highlight the IP address, click the Add § button to place payload markers around the IP address value of the `target_host` parameter:
    12. Switch to the Burp Intruder | Payloads tab and paste the 10 payloads you copied to the clipboard from the Burp Collaborator client into the Payload Options [Simple list] textbox using the Paste button: Make sure you uncheck the Payload Encoding checkbox.
    13. Click the Start attack button. The attack results table will pop up as your payloads are processing. Allow the attacks to complete. Note the [burpcollaborator.net](http://burpcollaborator.net/) URL is placed in the payload marker position of the `target_host` parameter:
    14. Return to the Burp Collaborator client and click the Poll now button to see whether any SSRF attacks were successful over any of the protocols. If any requests leaked outside of the network, those requests will appear in this table along with the specific protocol used. If any requests are shown in this table, you will need to report the SSRF vulnerability as a finding. As you can see from the results shown here, numerous DNS queries were made by the application on behalf of the attacker-provided payloads:
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89b351fe-d930-4215-8892-685f13a3ad6a/Untitled.png)
        
    
    ### How it works...
    
    Network leaks and overly-generous application parameters can allow an attacker
    to have an application make unauthorized calls via various protocols on the
    attacker's behalf. In the case of this recipe, the application allows DNS queries to
    leak outside of the local machine and connect to the internet
    
- **Testing Cross-Origin Resource Sharing (CORS)**
    
    ### How to do it...
    
    1. Navigate to HTML5 | Asynchronous JavaScript and XML | Pen Test Tool Lookup (AJAX):
    2. Select a tool from the listing and click the Lookup Tool button:
    3. Switch to the Burp Proxy | HTTP history tab and find the request you just made from the AJAX Version Pen Test Tool Lookup page. Flip to the Response tab:
    4. Let's examine the headers more closely by selecting the Headers tab of the same Response tab. Though this is an AJAX request, the call is local to the application instead of being made to a cross-origin domain. Thus, no CORS headers are present since it is not required. However, if a call to an external domain were made (for example, Google APIs), then CORS headers would be required:
    5. In an AJAX request, there is a call out to an external URL (for example, a cross-domain). In order to permit the external domain to receive DOM information from the user's browser session, CORS headers must be
    present, including **`Access-Control-Allow-Origin: <name of cross domain>.`**
    6. In the event the CORS header does not specify the name of the external domain and, instead, uses a wild card (*), this is a vulnerability. Web pentesters should include this in their report as a misconfigured CORS headers vulnerability.
    
    ### How it works...
    
    Since the AJAX call used in this recipe originated from the same place, there is no need for CORS headers. However, in many cases, AJAX calls are made to external domains and require explicit permission through the HTTP response
    Access-Control-Allow-Origin header
    
- **Performing Java deserialization attacks**
    
    ### How to do it...
    
    1. Switch to Burp BApp Store and install the Java Serial Killer plugin:
    In order to create a scenario using a serialized object, we will take a
    standard request and add a serialized object to it for the purposes of
    demonstrating how you can use the extension to add attacker-controlled
    commands to serialized objects.
    2. Note the new tab added to your Burp UI menu at the top dedicated to the
    newly-installed plugin.
    3. Navigate to the Mutillidae homepage.
    4. Switch to the Burp Proxy| HTTP history tab and look for the request you
    just created by browsing to the Mutillidae homepage:
    Unfortunately, there aren't any serialized objects in Mutillidae so we will
    have to create one ourselves.
    5. Switch to the Decoder tab and copy the following snippet of a serialized
    object:
    `AC ED 00 05 73 72 00 0A 53 65 72 69 61 6C 54 65`
    6. Paste the hexadecimal numbers into the Decoder tab, click the Encode as...
    button, and select base 64:
    7. Copy the base-64 encoded value from the Decoder tab and paste it into the
    bottom of the request you sent to the Java Serial Killer tab. Use Ctrl + C to
    copy out of Decoder and Ctrl + V to paste it anywhere in the white space
    area of the request:
    8. Within the Java Serial Killer tab, pick a Java library from the drop-down
    list. For this recipe, we will use CommonsCollections1. Check the Base64
    Encode box. Add a command to embed into the serialized object. In this
    example, we will use the `nslookup 127.0.0.1` command. Highlight the
    payload and click the Serialize button:
    9. After clicking the Serialize button, notice the payload has changed and now
    contains your arbitrary command and is base-64 encoded:
    10. Click the Go button within the Java Serial Killer tab to execute the payload.
    Even though you may receive an error in the response, ideally, you would
    have a listener, such as `tcpdump`, listening for any `DNS lookups on port 53.`
    From the listener, you would see the DNS query to the IP address you
    specified in the `nslookup` command.
        
        ### How it works...
        
        In cases where application code receives user input directly into an object
        without performing sanitization on such input, an attacker has the opportunity to
        provide arbitrary commands. The input is then serialized and run on the
        operating system where the application resides, creating a possible attack vector
        for remote code execution
