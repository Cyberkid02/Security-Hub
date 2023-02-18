# CH8:Evaluating Input Validation Checks

In this chapter, we will cover the following recipes:

- **Testing for reflected cross-site scripting**
    
    1. Switch to **Burp Proxy | HTTP** history and find the HTTP message you `justcreated` by selecting the lookup tool. Note that in the request is a parameter called `ToolID`. In the following example, the value is 16:
    
    1. Flip over to the Response tab and note the JSON returned from the request. You can find the JavaScript function in the response more easily by typing PenTest in the search box at the bottom. Note that the tool_id is reflected in a response parameter called toolIDRequested. This may be an attack vector for XSS:
    2. Send the request over to Repeater. Add an XSS payload within the ToolID parameter immediately following the number. Use a simple payload such as `<script>alert(1);</script>`:
    3. Click Go and examine the returned JSON response, searching for PenTest. Notice our payload is returned exactly as inputted. It looks like the developer is not sanitizing any of the input data before using it. Let's exploit the flaw:
    4. Since we are working with JSON instead of HTML, we will need to adjust the payload to match the structure of the JSON returned. We will fool the JSON into thinking the payload is legitimate. We will modify the original `<script>alert(1);</script>` payload to `"}} )%3balert(1)%3b//` instead.
    5. Switch to the Burp Proxy | Intercept tab. Turn Interceptor on with the button Intercept is on.
    6. While Proxy | Interceptor has the request paused, insert the new payload of "}} )%3balert(1)%3b// immediately after the Tool ID number:
    7. Click the Forward button. Turn Interceptor off by toggling to Intercept is off.
    8. Return to the Firefox browser and see the pop-up alert box displayed. You've successfully shown a proof of concept `(PoC)` for the reflected XSS vulnerability:
- **Testing for stored cross-site scripting**
    1. Place some verbiage into the text area. Before clicking the Save Blog Entry button, let's try a payload with the entry:
    2. Switch to the Burp Proxy | Intercept tab. Turn Interceptor on with the button Intercept is on.
    3. While Proxy | Interceptor has the request paused, insert the new payload of <script>alert(1);</script> immediately following the verbiage you dded to the blog:
    4. Click the Forward button. Turn Interceptor off by toggling to Intercept is off.
    5. Return to the Firefox browser and see the pop-up alert box displayed:
    6. Click the OK button to close the pop-ups. Reload the page and you will see the alert pop-up again. This is because your malicious script has become a permanent part of the page. You've successfully shown a proof of concept (PoC) for the stored XSS vulnerability!
- **Testing for HTTP verb tampering**
    1. Navigate to the homepage of OWASP Mutillidae II.
    2. Switch to Burp Proxy | HTTP history and look for the HTTP request you just created while browsing to the homepage of Mutillidae. Note the method used is GET. Right-click and send the request to Intruder:
    3. In the Intruder | Positions tab, clear all suggested payload markers. Highlight the GET verb, and click the Add $ button to place payload markers around the verb:
    4. In the Intruder | Payloads tab, add the following values to the Payload Options [Simple list] text box:
    
    ---
    
    ```jsx
    OPTIONS
    HEAD
    POST
    PUT
    DELETE
    TRACE
    TRACK
    CONNECT
    PROPFIND
    PROPPATCH
    MKCOL
    COPY
    ```
    
    1. Uncheck the Payload Encoding box at the bottom of the Payloads page and then click the Start attack button.
    2. When the attack results table appears, and the attack is complete, note all of the verbs returning a status code of 200. This is worrisome as most web servers should not be supporting so many verbs. In particular, the support for TRACE and TRACK would be included in the findings and final report as vulnerabilities:
- **Testing for HTTP Parameter Pollution**
    1. From the OWASP Mutilliae II menu, select Login by navigating to OWASP 2013 | A1 - Injection (Other) | HTTP Parameter Pollution | Poll Question:
    2. Select a tool from one of the radio buttons, add your initials, and click the Submit Vote button:
    3. Switch to the Burp Proxy | HTTP history tab, and find the request you just performed from the User Poll page. Note the parameter named choice. The value of this parameter is Nmap. Right-click and send this request to Repeater:
    4. Switch to the Burp Repeater and add another parameter with the same name to the query string. Let's pick another tool from the User Poll list and append it to the query string, for example, “&choice=tcpdump”. Click Go to send the request:
    5. Examine the response. Which choice did the application code accept? This is easy to find by searching for the Your choice was string. Clearly, the duplicate choice parameter value is the one the application code accepted to count in the User Poll vote:
- **Testing for SQL injection**
    1. At the Login screen, place invalid credentials into the username and
    password text boxes. For example, username is tester and password
    is tester. Before clicking the Login button, let's turn on Proxy |
    Interceptor.
    2. Switch to the Burp Proxy | Intercept tab. Turn the Interceptor on by toggling
    to Intercept is on.
    3. While Proxy | Interceptor has the request paused, insert the new payload of
    ' or 1=1--<space> within the username parameter and click the Login
    button:
    4. Click the Forward button. Turn Interceptor off by toggling to Intercept is
    off.
    5. Return to the Firefox browser and note you are now logged in as admin!
- **Testing for command injection**
    1. From the OWASP Mutilliae II menu, select DNS Lookup by navigating to
    OWASP 2013 | A1-Injection (Other) | Command Injection | DNS Lookup:
    2. On the DNS Lookup page, type the IP address 127.0.0.1 in the text box and click the Lookup DNS button:
    3. Switch to the Burp Proxy | HTTP history tab and look for the request you just performed. Right-click on Send to Intruder:
    4. In the Intruder | Positions tab, clear all suggested payload markers with the Clear $ button. In the target_host parameter, place a pipe symbol (|) immediately following the 127.0.0.1 IP address. After the pipe symbol,
    place an X. Highlight the X and click the Add $ button to wrap the X with payload markers:
    5. In the Intruder | Payloads tab, click the Load button. Browse to the location where you downloaded the SecLists-master wordlists from GitHub. Navigate to the location of the FUZZDB_UnixAttacks.txt wordlist and use
    the following to populate the Payload Options [Simple list] box: SecListsmaster |Fuzzing | FUZZDB_UnixAttacks.txt
    6. Uncheck the Payload Encoding box at the bottom of the Payloads tab page and then click the Start Attack button.
    7. Allow the attack to continue until you reach payload 50. Notice the responses through the Render tab around payload 45 or so. We are able to perform commands, such as id, on the operating system, which displays the results of the commands on the web page:
