# CH7:Assessing Business Logic

In this chapter, we will cover the following recipes:

- **Testing business logic data validation**
    1. Click **Concurrency | Shopping Cart Concurrency Flaw <this is your target lab>**
    2. Add **`1**` to the Quantity box for the **Sony - Vaio** **with Intel Centrino** item. Click the **Update Cart button**:
    3. Switch to **Burp Proxy | HTTP history tab**. Find the cart request, right-click, and click **Send to Repeater**:
    4. Inside Burp's **Repeater** tab, change the **`QTY3 parameter`** from `1 to 10:`
    5. Stay in Burp **Repeater**, and in the request pane, right-click and select **Request in browser | In current browser session:**
    6. A pop-up displays the modified request. Click the Copy button:
    7. Using the same Firefox browser containing the shopping cart, open a new tab and paste in the URL that you **copied into the clipboard** in the previous step:
    8. Press the Enter key to see the request resubmitted with a modified **quantity of** `10`:
    9. Switch to the original tab containing your shopping cart (the cart with the **original quantity** **of `**1`). Click the Purchase button:
    10. At the next screen, before clicking the Confirm button, switch to the second tab, and update the cart again, but this time with our **new quantity** of `10`, and click on Update Cart:
    11. Return to the first tab, and click the Confirm button: Notice we were able to purchase `10 Sony Vaio` laptops for the price of
- **Unrestricted file upload – bypassing weak validation**
    1. Open **File Upload Page** 
    2. Note the page instructs users to only **upload images**. If we try another type of file other than a `JPG **image**`, we receive an error message in the upper left-hand corner:
    3. On your local machine, create a file of any type, other than JPG. For example, create a Microsoft Excel file called `malicious_spreadsheet.xlsx`. It does not need to have any content for the purpose of this recipe.
    4. Switch to **Burp's Proxy | Intercept tab**. Turn Interceptor on with the button Intercept is on.
    5. Return to Firefox, and use the Browse button to find the **malicious_spreadsheet.xlsx file** on your system and click the Upload button:
    6. With the request paused in Burp's Proxy | Interceptor, change the `Contenttype from application/vnd.openxmlformatsofficedocument.spreadsheet.sheet` to `image/jpeg` instead. Here is the original: 
        
        []()
        
        Here is the modified version:
        
        []()
        
    7. Click the Forward button. Now turn Interceptor off by clicking the toggle button to Intercept is off.
    8. Note the file uploaded successfully! We were able to bypass the weak data validation checks and **upload a file** other than an **image**:
- **Performing process-timing attacks**
    
    By monitoring the time an application takes to complete a task, it is possible for attackers to **gather or infer information** about how an **application is coded**. 
    
    For example, a **login process** using **valid credentials** receives a **response quicker** than the same login process given **invalid credentials**. This delay in response time leaks information related to **system processes**. An attacker could use a **response time** to perform **account enumeration** and **determine valid usernames based upon** the **time** of the **response**.
    
    1. Go to the login page and log in using the username `ed` and the password  `pentest`.
    2. Switch to **Burp's Proxy | HTTP history tab**, find the login you just performed, right-click, and select Send to `**Intruder**`:
    3. Go to the **Intruder | Positions tab**, and clear all the payload markers, using the `Clear §` button on the right-hand side:
    4. Select the **password field** and click the `Add §` button to wrap a payload marker around that field:
    5. Also, **remove the PHPSESSID token**. Delete the value present in this token (the content following the equals sign) and leave it blank. This step is very important, because if you happen to leave this token in the requests, you will be unable to see the difference in the timings, since the application will think you are already logged in:
    6. Go to the **Intruder | Payloads tab**. Within the Payload Options [Simple list], we will add some invalid values by using a wordlist from `wfuzz` containing common passwords: `wfuzz | wordlists | other | common_pass.txt`:
    7. Scroll to the bottom and uncheck the checkbox for **Payload Encoding:**
    8. Click the Start attack button. An attack results table appears. Let the attacks complete. From the attack results table, select `Columns and check Response received`. Check **`Response completed`** to add these columns to the attack results table:
    9. Analyze the results provided. Though not obvious on every response, note
    the delay when an invalid password is used such as administrator. The
    Response received timing is 156, but the Response completed timing is
    10. However, the valid password of pentest (only 302) receives an immediate response: 50 (received), and 50 (completed):
    
    []()
    
- **Testing for the circumvention of workflows**
    
    Shopping cart to payment gateway interactions must be tested by web app penetration testers to ensure the workflow cannot be performed out of sequence. A payment should never be made unless a verification of the cart contents is checked on the server-side first. In the event this check is missing, an attacker can change the price, quantity, or both, prior to the actual purchase.
    
    1. You are presented with a shopping cart:
    2. Switch to **Burp's Proxy | HTTP history tab**, Click the Filter button, and ensure your Filter by **MIME type section includes Script**. If Script is not checked, be sure to check it now:
    3. Return to the Firefox browser and specify a quantity of `2` for the **Hewlett-Packard - Pavilion Notebook** with Intel Centrino item:
    4. Switch back to **Burp's Proxy | HTTP history tab** and notice the JavaScript (`*.js`) files associated with the change you made to the quantity. Note a script called clientSideValiation.js. Make sure the status code is `200`
    and not `304` (not modified). Only the `200` status code will show you **the source code of the script**:
    5. Select the `clientSideValidation.js` file and view its source code in the Response tab.
    6. Note that **coupon codes** are hard-coded within the JavaScript file. However, used literally as they are, they will **not work**:
    7. Keep looking at the source code and notice there is a **decrypt function** found in the JavaScript file. We can test one of the **coupon codes** by sending it through this **function**. Let’s try this test back in the Firefox browser:
    8. In the browser, bring up the developer tools **(F12)** and go to the Console tab. Paste into the console (look for the >> prompt) the following command: `decrypt('emph');`
    9. You may use this command to call the **decrypt function** on any of the coupon codes declared within the array:
    10. after pressing enter the coupon will be decrypted
    11. Place the word GOLD within the Enter your coupon code box. Notice the amount is now much less. Next, click the Purchase button:
    12. We receive confirmation regarding **stage 1 completion**. Let's now try to get the **`purchase for free`**:
    13. Switch to **Burp's Proxy | Intercept** **tab** and turn Interceptor on with the button **Intercept is on.**
    14. Return to Firefox and press the Purchase button. While the request is paused, modify the `$1,599.99` amount to `$0.00`. Look for the `GRANDTOT` parameter to help you find the grand total to change:
    15. Click the Forward button. Now turn Interceptor off by clicking the toggle button to Intercept is off.
    16. You should receive a **success message**. Note the total charged is now `$0.00:`
- **Uploading malicious files – polyglots**
    
    `Polyglot` is a term defined as something that uses several languages. If we carry this concept into hacking, it means the creation of a cross-site scripting (XSS) attack vector by using different languages as execution points. For example, attackers can construct valid images and embed JavaScript with them. The placement of the JavaScript payload is usually in the comments section of an image. Once the image is loaded in a browser, the XSS content may execute, depending upon the strictness of the content-type declared by the web server and the interpretation of the content-type by the browser.
    
    1. Click Malicious Execution | Malicious File Execution from the left-hand menu. You are presented with a file upload functionality page. The instructions state that only images are allowed for upload:
    2. Browse to the location where you saved the `xss.jpg` image that you downloaded from the `PortSwigger` blog page mentioned at the beginning of this recipe.
    3. The following screenshot how the image looks. As you can see, it is difficult to detect any XSS vulnerability contained within the image. It is hidden from plain view.
        
        
    4. Click the Browse button to select the `xss.jpg` file:
    5. Switch to **Burp's Proxy | Options**. Make sure you are capturing Client responses and have the following settings enabled. This will allow us to **`capture HTTP responses modified** or **intercepted**`:
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26b671f3-4069-424c-9343-4e39997f3841/Untitled.png)
        
    6. Switch to **Burp's Proxy | Intercept** tab. Turn Interceptor on with the button Intercept is on.
    7. Return to the Firefox browser, and click the **Start Upload button**. The message should be paused within Burp's Interceptor.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b8e3c1d4-4c3b-4dd1-a78c-fda3e17d8d4d/Untitled.png)
        
    8. Within the Intercept window while the request is paused, type Burp rocks into the search box at the bottom. You should see a match in the middle of the image. This is our polyglot payload. It is an image, but it contains a `hidden XSS script` within the comments of the image:
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89a0ff73-6187-49fa-8e8b-61eaea264c07/Untitled.png)
        
    9. Using Notepad or your favorite text editor, create a new file called `poly.jsp`, and write the following code within the file:
        
        ```jsx
        <HTML>
        <% java.io.File file = new
        java.io.File("/var/lib/tomcat6/webapps/WebGoat/mfe_target/guest.txt");
        file.createNewFile();%>
        </HTML>
        ```
        
    10. Return to the **Malicious File Execution** page, and browse to the `poly.jsp` file you created, and then click the Start Upload button. The `poly.jsp` is a Java Server Pages file that is executable on this web server. Following the instructions, we must create a guest.txt file in the path provided. This code creates that file in `JSP` **scriptlet** tag code:
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a5e44be-b0d0-4e7d-9e31-5bbb0d81f136/Untitled.png)
        
    11. Right-click the unrecognized image, and select Copy Image Location.
    12. Open a new tab within the same Firefox browser as `WebGoat`, and paste the image location in the new tab. Press Enter to execute the script, and give the script a few seconds to run in the background before moving to the next step.
    13. Flip back to the first tab, F5, to refresh the page, and you should receive the successfully completed message. If your script is running slowly, try uploading the `poly.jsp` on the upload page again. The success message should appear:
