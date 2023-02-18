# CH9:Attacking the Client

In this chapter, we will cover the following recipes:

- **Testing for Clickjacking**
    1. Navigate to the Home page of the OWASP Mutillidae II.
    2. Switch to Burp, and from the top-level menu, select Burp `Clickbandit`:
    3. A pop-up box explains the tool. Click the button entitled Copy `Clickbandit` to clipboard:
    4. Return to the Firefox browser, and press F12 to bring up the developer tools. From the developer tools menu, select Console, and look for the prompt at the bottom:
    5. At the Console prompt (for example, >>), paste into the prompt the `Clickbandit` script you copied to your clipboard:
    6. After pasting in the script into the prompt, press the Enter key. You should see the Burp `Clickbandit` Record mode. Click the Start button to begin:
    7. Start clicking around on the application after it appears. Click available links at the top Mutillidae menu, click available links on the side menu, or browse to pages within Mutillidae. Once you've clicked around, press the Finish button on the Burp Clickbandit menu.
    8. You should notice big red blocks appear transparently on top of the Mutillidae web pages. Each red block indicates a place where a malicious iframe can appear. Feel free to click each red block to see the next red block appear, and so on:
    9. Once you wish to stop and save your results, click the Save button. This will save the Clickjacking PoC in an HTML file for you to place inside your penetration test report.
- **Testing for DOM-based cross-site scripting**
    1. Navigate to OWASP 2013 | HTML5 Web Storage | HTML5 Storage:
    2. Note the name/value pairs stored in the DOM using HTML5 Web Storage locations. Web storage includes Session and Local variables. Developers use these storage locations to conveniently store information inside a user's browser:
    3. Switch to the Burp Proxy Intercept tab. Turn Interceptor on with the button Intercept is on.
    4. Reload the HTML 5 Web Storage page in Firefox browser by pressing F5 or clicking the reload button.
    5. Switch to the Burp Proxy HTTP history tab. Find the paused request created by the reload you just performed. Note that the User-Agent string is highlighted, as shown in the following screenshot:
    6. Replace the preceding highlighted User-Agent with the following script:
        
        ```jsx
        <script>try{var m = "";var l = window.localStorage; var s =
        window.sessionStorage;for(i=0;i<l.length;i++){var lKey = l.key(i);m += lKey + "=" + l.getItem(lKey) + ";\n";};for(i=0;i<s.length;i++){var lKey = s.key(i);m += lKey "=" + s.getItem(lKey) + ";\n";};alert(m);}catch(e){alert(e.message);}
        </script>
        ```
        
    7. Click the Forward button. Now, turn Interceptor off by clicking the toggle
    button to Intercept is off.
    8. Note the alert popup showing the contents of the DOM storage:
- **Testing for JavaScript execution**
    1. Note after clicking the Generate Password button, a password is shown.
    Also, note the username value provided in the URL is reflected back as
    is on the web page: `[http://192.168.56.101/mutillidae/index.php](http://192.168.56.101/mutillidae/index.php)?page=password generator.php&username=anonymous`. This means a potential XSS vulnerability may exist on the page:
    2. Switch to the Burp Proxy HTTP history tab and find the HTTP message associated with the Password Generator page. Flip to the Response tab in the message editor, and perform a search on the string catch. Note that the
    JavaScript returned has a catch block where error messages display to the user. We will use this position for the placement of a carefully crafted JavaScript injection attack:
    3. Switch to the Burp Proxy Intercept tab. Turn Interceptor on with the button Intercept is on.
    4. Reload the Password Generator page in Firefox browser by pressing F5 or clicking the reload button.
    5. Switch to the Burp Proxy Interceptor tab. While the request is paused, note the username parameter value highlighted as follows:
    6. Replace the preceding highlighted value of anonymous with the following carefully crafted JavaScript injection script:
    `canary";}catch(e){}alert(1);try{a="`
    7. Click the Forward button. Now, turn Interceptor off by clicking the toggle button to Intercept is off.
    8. Note the alert popup. You’ve successfully demonstrated the presence of a JavaScript injection XSS vulnerability
- **Testing for HTML injection VIA COOKIE**
    
    1. Navigate to OWASP 2013 | A1 – Injection (Other) | HTMLi Via Cookie Injection | Capture Data Page:
    2. Note how the page looks before the attack:
    
    []()
    
    3. Switch to the Burp Proxy Intercept tab, and turn Interceptor on with the button Intercept is on.
    4. While the request is paused, make note of the last cookie, `acgroupswitchpersist=nada:`
    5. While the request is paused, replace the value of the last cookie, with this HTML injection script:
    
    ```jsx
    <h1>Sorry, please login again</h1><br/>Username<input
    type="text"><br/>Password<input type="text"><br/><input
    type="submit" value="Submit"><h1>&nbsp;</h1>
    ```
    
    6. Click the Forward button. Now turn Interceptor off by clicking the toggle button to Intercept is off.
    7. Note how the HTML is now included inside the page!
    
- **Testing for client-side resource manipulation (open redirect)**
    1. Navigate to OWASP 2013 | A10 – Unvalidated Redirects and Forwards | Credits:
    2. Click the `ISSA Kentuckiana` link available on the Credits page:
    3. Switch to the Burp Proxy HTTP history tab, and find your request to the Credits page. Note that there are two query string parameters: `page` and `forwardurl`. What would happen if we manipulated the URL where the user is sent?
    4. Switch to the Burp Proxy Intercept tab. Turn Interceptor on with the button Intercept is on.
    5. While the request is paused, note the current value of the `fowardurl` parameter:
    6. Replace the value of the `forwardurl` parameter to be [https://www.owasp.org](https://www.owasp.org/) instead of the original choice of [http://www.issa-kentuckiana.org](http://www.issa-kentuckiana.org/):
    7. Click the Forward button. Now turn Interceptor off by clicking the toggle button to Intercept is off.
    8. Note how we were redirected to a site other than the one originally clicked!
