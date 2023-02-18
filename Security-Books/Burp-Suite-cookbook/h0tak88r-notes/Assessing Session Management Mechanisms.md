# CH6:Assessing Session Management Mechanisms

In this chapter, we will cover the following recipes:

- **Testing session token strength using Sequencer**.
    1. Open the Firefox browser to access the home page of `OWASP Mutillidae II`
    2. Switch to the **Proxy | HTTP History tab** and select the request showing your initial browse to the `Mutillidae` home page.
    3. Look for the **GET request** and the associated response containing the `SetCookie: assignments`. Whenever you see this assignment, you can ensure you are getting a freshly created cookie for your session. Specifically, we are interested in the `PHPSESSID cookie value:`
    4. Highlight the value of the of the `PHPSESSID cookie`, right-click, and select `Send to Sequencer`: Sequencer is a tool within Burp designed to determine the strength or the **quality of the randomness created within a session token**.
    5. After sending the value of the `PHPSESSID` parameter over to Sequencer, you will see the value loaded in the Select Live Capture Request table.
    6. Before pressing the Start live capture button, scroll down to the Token Location Within Response section. In the Cookie dropdown list, select
    `PHPSESSID=<captured-session-token-value>:`
    7. Since we have the correct cookie value selected, we can begin the live capture process. Click the Start live capture button, and Burp will send multiple requests, extracting the `PHPSESSID cookie` out of each response.
    After each capture, `Sequencer` performs a statistical analysis of the level of randomness in each token.
    8. Allow the capture to gather and **analyze at least 200 tokens**, but feel free to let it run longer if you like.
    9. Once you have at least **200 samples**, click the Analyze now button.
    Whenever you are ready to stop the capturing process, press the Stop button
    and confirm Yes:
    10. After the analysis is complete, the output of Sequencer provides an overall result. In this case, the quality of randomness for the PHPSESSID session token is excellent. The amount of effective entropy is estimated to be 112 bits. From a `web pentester` perspective, these session tokens are very strong, so there is no vulnerability to report here. However, though there is no vulnerability present, it is good practice to perform such checks on session tokens:
    
    []()
    
- **Testing for cookie attributes**
    
    look for secure attributes like **`httponly`**
    
    `Set-Cookie: PHPSESSID=<session token value>;path=/;Secure;**HttpOnly;**`
    
- **Testing for session fixation**
    
    In this recipe, we examined how the PHPSESSID value assigned to an unauthenticated user remained constant even after authentication. This is a security vulnerability allowing for the session fixation attack
    
    - Send two requests to comparer one is **unauthenticated and the other authenticated**
    - Note the value of `PHPSESSID` does not change between the **unauthenticated session** (on the left) and the **authenticated session** (on the right). This means the application has a **`session fixation vulnerability`**:
- **Testing for exposed session variables**
    
    As seen in this recipe, there isn't anything hidden about hidden form fields. As penetration testers, we should examine and manipulate these values, to determine whether sensitive information is, inadvertently, exposed or whether we can change the behavior of the application from what/ is expected, based on our role and authentication status. In the case of this recipe, we were not even logged into the application. We manipulated the hidden form field labeled page to access a page containing fingerprinting information. Access to such information should be protected from unauthenticated users
    
- **Testing for Cross-Site Request Forgery**
    
    **How it works...**
    CSRF attacks require an authenticated user session to surreptitiously perform
    actions within the application on behalf of the attacker. In this case, an attacker
    rides on it's session to re-run the registration form, to create an account for the
    attacker. If it had been an admin, this could have allowed the account role to be
    elevated as well.
    
    - Open Vulnerable page
    - send request to repeater
    - If you're using Burp Professional, right-click **select Engagement** **tools |Generate** `CSRF PoC:`
