# CH4:Assessing Authentication Schemes

In this chapter, we will cover the following recipes:

- **Testing for account enumeration and guessable accounts**
    1. Click the Log In button, and at the login screen, attempt to log in with an account username of **`admin`** and a password of **`aaaaa`**:
    2. Note the message returned is The `**password is invalid`** . From this information, we know **admin is a valid account**. Let's use Burp Intruder to find more accounts.
    3. In Burp's **Proxy** | **HTTP history** tab, find the **failed login attempt message**.
    View the **Response** | **Raw** tab to find the same overly verbose error message, `**The password is invalid:**`
    4. Flip back to the **Request** | **Raw** tab and right-click to send this request to **Intruder**:
    5. Go to Burp's **Intruder** | **Positions** tab. Notice how Burp places payload markers around each parameter value found. However, we only need a payload marker around the **password value**. Click the **Clear §** button to remove the payload markers placed by Burp , Then **highlight** the name value of `**admin**` with your cursor and click the **Add §** button:
    6. Continue to the **Intruder | Payloads tab**. Many testers use word lists to enumerate commonly used usernames within the payload marker placeholder.
    7. Go to the **Intruder | Options tab** and scroll down to the **`Grep – Match`** section. Click the checkbox Flag **result items with responses matching these expressions**. Click the Clear button to remove the items currently in the list:
    
    8. Type the string The `**password is invalid**` within the **textbox** and click the **Add button.**
    9. Click the **Start attack button** . A popup dialog box appears displaying the payloads defined, as well as the new
    column we added under the **`Grep – Match` section**. This pop-up window is the attack results table.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/21f4f927-1054-41e6-acaa-bb64eed23ebb/Untitled.png)
    
    1. The attack results table shows each request with the given payload resulted
    in a status code of `200` and that two of the payloads, john and tom, did not produce the message The password is invalid within the responses. Instead, those two payloads returned a message of `**The user does not exist:**`
    2. The result of this attack results table provide a `**username enumeration vulnerability**` based upon the overly verbose error message `**The password is invalid**`, which confirms the user account exists on the system: This means we are able to confirm that accounts already exist in the system for the users `**user, demo, and admin**`
- **Testing for weak lock-out mechanisms**
    1. At the login screen, attempt to login five times with username `**admin**` and the wrong password of `**aaaaaa**`. Notice the **application does not react any**
    2. differently during the five attempts. The application **`does not change the error message shown`**, and **`the admin account is not locked out`**. This means the **login is probably susceptible to brute-force password-guessing attacks**:
    Let's continue the testing, to brute-force the login page and gain **unauthorized access to the application**.
    3. Go to the **Proxy | HTTP history tab**, and look for the failed login attempts. Right-click one of the five requests and send it to Intruder:
    4. Go to Burp's **Intruder** tab, and leave the **Intruder | Positions tab** and notice how Burp places
    payload markers around each parameter value found. However, we only need a payload marker around the **password's value**. Click the **Clear§ button** to remove the payload markers placed by Burp Then, highlight the password value of **`aaaaaa`** and click the **Add § button**.
    5. Continue to the **Intruder | Payloads tab**. Many testers use word lists to
    brute-force commonly used passwords within the payload marker
    placeholder.
    6. Go to the **Intruder | Options tab** and scroll down to the `**Grep – Extract**`
    section
    7. Click the **checkbox Extract the following items from responses** and then click the **Add button**. A pop-up box appears, displaying the **response** of the unsuccessful login attempt you made with the `**admin/aaaaaa**` request.
    8. In the **search box** at the bottom, search for the words `**Not Logged In**`. After finding the match, you must highlight the words Not Logged In, to assign the **`grep match`** correctly
    9. Now, click the Start attack button at the top right-hand side of the Options page. A pop-up attack results table appears, displaying the request with the payloads you defined placed into the payload marker positions. Notice the
    attack table produced shows an extra column entitled `**ReflectedXSSExecution**`. This column is a result of the **`Grep – Extract`** Option set previously.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7f49e323-7383-4bc9-afc5-319a625656af/Untitled.png)
    
    1. From this attack table, viewing the additional column, a tester can easily identify which request number successfully brute-forced the login screen. In
    2. this case, Request 4, using credentials of the **`username admin`** and the
    `**password admin**` logged us into the application
    3. Select Request 4 within the attack table, and view the **Response | Render** tab.
    
    You successfully brute-forced the password of a valid account on the system, due to the application having a weak lock-out mechanism.
    
- **Testing for bypassing authentication schemes**
    1. Open the Firefox browser to the home page of your target , using the Home button from the top menu, on the left-hand side. Make sure you are not logged into the application. If you are logged in, select Logout from the menu:
    2. In Burp, go to the **Proxy | HTTP history** **tab** and select the request you just made, browsing to the **home page as unauthenticated**. Right-click, and then select Send to **`Repeater`**:
    3. Using this same request and location, right-click again, and then select Send to **`Comparer (request):`**
    4. Return to the home page of your browser and click the Login/Register button. At the login page, log in with the username of `**admin**` and the password of `**admin**`. Click Login.
    5. After you log in, go ahead and **log out**. Make sure you press the **Logout button** and are logged out of the **admin account.**
    6. In Burp, go to the **Proxy | HTTP** **history tab** and select the request you just made, **logging in as admin**. Select **GET request immediately following the POST 302 redirect**. Right-click and then select Send to `Repeater (request):`
    7. Using this same request and location, right-click again and Send to `Comparer (request):`
    8. Go to Burp's Comparer tab. Notice the two requests you sent are highlighted. Press the Words button on the bottom right-hand side, to `compare the two requests at the same time:`
    9. A dialog pop-up displays the two requests with **color-coded highlights** to draw your eyes to the differences. Note the changes in the `Referer header` and the `additional name/value pair` placed in the **admin account cookie**.            **[note that in your scenario it might not be the same the point here is to spot the difference]**
    10. Return to **`Repeater`**, which contains your first GET request you performed **as unauthenticated**. Prior to performing this attack, make sure you are completely logged out of the application.
    11. Now flip over to the `Repeater` tab, which contains your second GET request as **authenticated user admin**. Copy the values for `Referer` header and Cookie from the authenticated request. This attack is parameter modification for the purpose of **bypassing authentication**:
    12. Copy the highlighted headers **(Referer and Cookie) from the authenticated GET request**. You are going to paste those values into the **unauthenticated GET request.**
    13. Click the Go button to **send your modified GET request**. Remember, this is the first GET request you performed as **unauthenticated.**
    14. Verify that you are **now logged in as `admin**` in the **Response | Render tab**. We were able to **bypass the authentication mechanism** (that is, the log in page) by performing parameter manipulation:
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27723877-f51f-4b80-ace7-e8ca452e792e/Untitled.png)
    
- **Testing for browser cache weaknesses**
    1. Log into the target application as `admin` with the password `admin`.
    2. Now log out of the application by clicking the Logout button from the top menu.
    3. Verify you are logged out by noting the Not Logged In message.
    4. View these steps as messages in Burp's **Proxy | History** as well. Note the logout performs a 302 redirect in an effort to not cache cookies or credentials in the browser:
    5. From the Firefox browser, click the back button and notice that you are now logged in as `admin` even though you did not `log in!` This is possible because of cached credentials stored in the browser and the lack of any cachecontrol protections set in the application.
    6. Now `refresh/reload` the page in the browser, and you will see you are `logged out again`.
    7. Examine the steps within the Proxy | HTTP history tab. 
    8. Review the steps you did through the browser against the messages captured in the **Proxy | HTTP history** **table**:
        
        ```jsx
        Request 1 in the following screenshot is unauthenticate
        Request 35 is the successful login (302) as admin
        Request 37 is the logout of the admin account
        Requests 38 and 39 are the refresh or reload of the browser page,
        logging us out again
        ```
        
    9. There is no request captured when you press the browser's back button.
    10. This is because the back button action is contained in the browser. No message was sent through Burp to the web server to perform this action. This is an important distinction to note. Nonetheless, we found a vulnerability associated with weak browser-caching protection. In cases such as this, penetration testers will take a screenshot of the logged-in cached page, seen after clicking the back button:
- **Testing the account provisioning process via REST API**
    1. Within `Mutillidae`, browse to the User **Lookup (SQL) Page and select OWASP 2013 | A1 Injection (SQL) | SQLi – Extract** **Data | User Info (SQL):**
    2. Type user for Name and user for Password, and click View Account Details. You should see the results shown in the next screenshot. This is the account we will test provisioning functions against, using REST calls:
    3. Through `Spidering`, Burp can find `/api` or `/rest` folders. Such folders are clues that an application is `REST API` enabled. A tester needs to determine which functions are available through these API calls.
    4. For `Mutillidae`, the `/webservices/rest/` folder structure offers account provisioning through `REST API calls`.
    5. To go directly to this structure within `Mutillidae`, select Web Services **|REST | SQL Injection | User Account Management**: You are presented with a screen describing the supported `REST calls` and
    parameters required for each call:
    6. Let's try to invoke one of the **REST calls**. Go to the **Proxy | HTTP history** table and select the latest request you sent from the menu, to get to the **User Account Management page**. Right-click and send this request to `**Repeater**`:
    7. In Burp's Repeater, add the **?**, followed by a parameter name/value pair of `username=user` to the URL. The new URL should be as follows:
    `/mutillidae/webservices/rest/ws-user-account.php?username=user`
    8. Click the Go button and notice we are able to retrieve data as an `unauthenticated user!` **No authentication token is required to perform such actions:**
    9. Let's see what else we can do. Using the SQL Injection string given on the `User Account Management page`, let's attempt to dump the entire user table.
    10. Append the following value after username=:
        
        `user'+union+select+concat('The+password+for+',username,'+is+',+password),mysignature+from+accounts+--+`
        The new URL should be the following one:
        `/mutillidae/webservices/rest/ws-user-account.php?username=user'+union+select+concat('The+password+for+',userna
        me,'+is+',+password),mysignature+from+accounts+--+`
        
    11. Click the Go button after making the change to the username parameter.
    12. Notice we **dumped all of the accounts** in the database, displaying all
    usernames, passwords, and signatures:
    13. Armed with this information, return to **Proxy | HTTP History**, select the
    request you made to see the **User Account Management page**, right-click,
    and send to Repeater.
    14. In Repeater, **modify the GET verb and replace it with DELETE within the Raw tab of the Request:**
    15. Move to the `Params` tab, click the Add button, and add two Body type parameters: first, a username with the value set to user, and second, a password with the value set to user, and then click the Go button:
    16. Notice **we deleted the account!** We were able to retrieve information and even modify (delete) rows within the database without ever showing an `API key or authentication token!`
    Note: If you wish to re-create the user account, repeat the previous steps, replacing delete with put. A signature is optional. Click the Go button. The user account is re-created again.
