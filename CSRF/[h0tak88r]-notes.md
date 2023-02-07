## #What it is ??

**Cross-Site Request Forgery (CSRF/XSRF) is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated.** 

**CSRF attacks specifically target state-changing requests, not theft of data, since the attacker has no way to see the response to the forged request. - OWASP**

## #CSRF Workflow

![image](https://user-images.githubusercontent.com/108616378/217371342-091fc3ea-9b94-41de-b737-fb351badbb18.png)
# Basic Test

```python
1. Select a request anywhere in Burp Suite Professional that you want to test or exploit.
2. From the right-click context menu, select Engagement tools / Generate CSRF PoC.
3. Burp Suite will generate some HTML that will trigger the selected request (minus cookies, which will be added automatically by the victim's browser).
4. You can tweak various options in the CSRF PoC generator to fine-tune aspects of the attack. You might need to do this in some unusual situations to deal with quirky features of requests.
5. Copy the generated HTML into a web page, view it in a browser that is logged in to the vulnerable web site, and test whether the intended request is issued successfully and the desired action occurs.
```

# Checklist

- [ ]  **Change single char**
- [ ]  **Sending empty value of token Replace with same length**
- [ ]  **Clickjacking**
- [ ]  **Changing post/get method**
- [ ]  **Remove it from request**
- [ ]  **Use another user's valid token**
- [ ]  **Crsf protection by Referrer Header? Remove the header [ADD in form <meta name="referrer" content="no-reference">]**
- [ ]  **Bypass using subdomain [[victim.com.attacker.com](http://victim.com.attacker.com/)]**
- [ ]  **Try to decrypt hash (may be CSRF is a hash)**
- [ ]  **Analyze Token(use burp)**
- [ ]  **Gmail -> Mail send to [email+2=@gmail.com](mailto:email+2=@gmail.com) will actually send to [email@gmail.com](mailto:email@gmail.com)**
- [ ]  **CSRF tokens leveraging XSS vulnerabilities**
- [ ]  **Sometimes Anti-CSRF token is composed of two parts, one of them remains static while the other one dynamic."837456mzy29jkd911139" for one request the other time "837456mzy29jkd337221" if you notice, "837456mzy29jkd" part of the token remain same, send the request with only the static part**
- [ ]  **Sometimes anti-csrf check is dependent on User-Agent as well.**
- [ ]  **If you try to use mobile/tablet user agent, application may not even check for anti-csrf token.**
