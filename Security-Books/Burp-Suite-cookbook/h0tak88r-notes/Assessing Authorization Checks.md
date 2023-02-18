# CH5:Assessing Authorization Checks

In this chapter, we will cover the following recipes:

- **Testing for directory traversal**
    1. Open the Firefox browser on the login screen. From the top menu, click `Login`.
    2. Find the request you just performed within the Proxy | HTTP history table. Look for the call to the `login.php` page. Highlight the message, Send to `**Intruder:**`
    3. Switch over to the **Intruder | Positions tab**, and clear all Burp-defined payload markers by clicking the **Clear $ button** on the right-hand side. Highlight the value currently stored in the page parameter `(login.php)`, and place a payload marker around it using the **Add § button:**
    4. Continue to the **Intruder | Payloads tab**, and select the following wordlist from the `wfuzz` repository:
    `wfuzz/wordlist/general/admin-panels.txt.`
    5. Click the Load button within the Payload Options [Simple list] section of the **Intruder | Payloads,** tab and a popup will display, prompting for the location of **your wordlist**.
    6. Scroll to the bottom and **uncheck** (by default, it is checked) the option `URL-encode these characters`
    7. You are now ready to begin the attack. The attack results table will appear. Allow the attacks to complete. There are **137 payloads** in the **admin-panels.txt** wordlist. `Sort on the Length column` from ascending to descending order, to see which of the payloads **hit a web page**.
    8. Notice the payloads that have **`larger response lengths`**. This looks promising! Perhaps we have stumbled upon some **administration pages** that may contain **fingerprinting information or unauthorized access**:
    9. Select the first page in the list with the largest length, **`administrator.php`**. From the attack results table, look at the **Response | Render** **tab**, and notice the page displays the **PHP version** and the **system information:**
    
![image](https://user-images.githubusercontent.com/108616378/219857080-7d28d11b-62e4-45c2-aae2-424d34b92853.png)


- **Testing for Local File Include (LFI)**
    1. Open the Firefox browser to the **login screen** .
    2. Find the request you just performed within the **Proxy | HTTP history table**. Look for the call to the `**login.php**` page. Highlight the message and Send to **`Intruder`**.
    3. Switch over to the **Intruder | Positions** tab Highlight the value **currently stored in** the **page parameter `**(login.php)`, and place a payload marker around it using the **Add §** button on the right-hand side.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3b6af088-a079-48b4-881d-32c67305fed8/Untitled.png)
        
    4. Continue to the **Intruder | Payloads tab**. Select the following wordlist from the `wfuzz` repository: Traversal.txt. The location of the wordlist from the GitHub repository follows this folder structure:
    `wfuzz/wordlist/injections/Traversal.txt.`
    5. Browse to the location where you downloaded the `wfuzz` repository from GitHub. Continue to search through `wfuzz` folder structure until you reach the `traversal.txt` file. Select the file and click Open:
    6. Scroll to the bottom and uncheck (by default, it is checked) the option `URL-encode these characters`.
    7. The attack results table will appear. Allow the attacks to complete. Sort on the Length column from ascending to descending order, to see which of the payloads hit a web page. Notice the payloads with larger lengths; perhaps
    we gained **unauthorized access to the system** **configuration files**!
    8. Select the Request #2 in the list. From the attack results table, look at the **Response | Render tab** and notice the page displays the host file from the system!
    9. Continue scrolling down the list of requests in the attack results table. Look at request #6, and then look at the Response | Render tab and notice the page displays the `**/etc/passwd**` file from the **system**!
    
![image](https://user-images.githubusercontent.com/108616378/219857136-3234cfc1-318b-4e28-91c7-058dde77c6a3.png)

- **Testing for Remote File Include (RFI)**
    1. From the OWASP BWA Landing page, click the link to the `OWASP Mutillidae II` application.
    2. Open the Firefox browser to the login screen of `OWASP Mutillidae II`. From the top menu, click Login.
    3. Find the request you just performed within the Proxy | HTTP history table.
    Look for the call to the `login.php` page:
    4. Make a note of the page parameter that determines the page to load: Let's see if we can exploit this parameter by providing a URL that is outside the application. For demonstration purposes, we will use a URL that we control in the `OWASP BWA VM`. However, in the wild, this URL would be attacker-controlled instead.
    5. Switch to the **Proxy | Intercept tab**, and press the **Intercept is on button**.
    6. Return to the Firefox browser, and reload the **login page**. The request is paused and contained within the **Proxy | Intercept tab**
    7. Now let's manipulate the value of the page parameter from `login.php` to a URL that is external to the application. Let's use the login page to the`GetBoo` application. Your URL will be specific to your machine's IP address, so adjust accordingly. The new URL will be
    `http://<your_IP_address>/getboo/`
    8. Replace the `login.php` value with `http://<your_IP_address>/getboo/` and click the Forward button:
    9. Now press the **Intercept is on** again to toggle the intercept button to **OFF (Intercept is off).**
    10. Return to the Firefox browser, and notice the page loaded is the `GetBoo` index page within the context of the `Mutillidae application!`
- **Testing for privilege escalation**
    1. From the OWASP BWA Landing page, click the link to the **OWASP Mutillidae II** application.
    2. Open the Firefox browser to the login screen of **OWASP Mutillidae II**. From the top menu, click **Login**.
    3. At the **login screen**, log in with these credentials → **username: john and password: monkey.**
    4. Switch to Burp's **Proxy | HTTP history tab**. Find the POST and subsequent GET requests you just made by logging in as john:
    5. Look at the GET request from the listing; notice the **cookie name/value pairs** shown on the **Cookie: line**.
    The **name/value** pairs of most interest include **`username=john and uid=3**.` What if we attempt to manipulate these values to a different role?
    6. Let's attempt to manipulate the parameters username and the `uid` stored in the cookie to a different role. We will use **Burp's Proxy | Intercept** to help us perform this attack.
    7. Switch to the **Proxy | Intercept** tab, and press the Intercept is on button. Return to the Firefox browser and reload the **login page**.
    8. The request is paused within the Proxy | Intercept tab. While it is paused, change the value assigned to the username from **john to admin**. Also, change the value assigned to the `uid from 3 to 1:`
    9. Click the Forward button, and press the Intercept is on again to toggle the intercept button to OFF (Intercept is off).
    10. Return to the Firefox browser, and notice we are now logged in as an `**admin!**` We were able to escalate our privileges from a regular user to an `admin`, since the developer did not perform any **authorization checks** on the assigned role:
- **Testing for insecure direct object reference(IDOR)**
    1. From the Mutillidae menu, select **`OWASP 2013 | A4 – Insecure Direct Object`** References | Source Viewer:
    2. From the Source Viewer page, using the default file selected in the **dropdown box (upload-file.php)**, click the View File button to see the contents of the **file displayed** below the button:
    3. Switch to **Burp's Proxy | HTTP history tab**. Find the POST request you just made while viewing the `upload-file.php` file. Note the `phpfile` parameter with the value of the file to display. What would happen if we change the value of this parameter to something else?
    4. Let's perform an IDOR attack by manipulating the value provided to the `phpfile` parameter to reference a file on the system instead. For example, let's try changing the `upload-file.php` value to `../../../../etc/passwd` via **Burp's Proxy | Intercept** functionality.
    5. As the request is paused, change the value assigned to the `phpfile` parameter to the value `../../../../etc/passwd` instead:
    6. Click the Forward button. Now press the Intercept is on button again to toggle the intercept button to OFF (Intercept is off).
    7. Return to the Firefox browser. Notice we can now see the contents of the`/etc/passwd` file!
