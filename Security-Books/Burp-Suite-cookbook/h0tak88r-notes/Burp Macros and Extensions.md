# CH10:Working with Burp Macros and Extensions

In this chapter, we will cover the following recipes:

- **Creating session-handling macros**
    1. Navigate to the Login page in Mutillidae. Log into the application as username `ed` with password `pentest`.
    2. Immediately log out of the application by clicking the Logout button and make sure the application confirms you are logged out.
    3. Switch to the Burp Proxy HTTP history tab. Look for the logout request you just made along with the subsequent, unauthenticated GET request. Select the unauthenticated request, which is the second GET. Right-click and send that request to **Repeater.**
    4. Switch to Burp **Repeater**, then click the Go button. On the Render tab of the response, ensure you receive the Not Logged In message. We will use this scenario to build a session-handling rule to address the unauthenticated session and make it an authenticated one, as follows:
    5. Switch to the **Burp Project options tab**, then the **Sessions tab**, and click the **Add button** under the **Session Handling Rules section.**
    6. After clicking the Add button, a pop-up box appears. Give your new rule a name, such as `LogInSessionRule`, and, under **Rule Actions**, select **Run a macro**.
    7. Another pop-up box appears, which is the Session handling action editor. In the first section, under Select macro, click the **Add button.**
    8. After clicking the Add button, the macro editor appears along with another pop-up of the **Macro Recorder,**
    → Note: `A bug` exists in `1.7.35` that disables Macro Recorder. Therefore, after clicking the Add button, if the recorder does not appear, upgrade the Burp version to `1.7.36` or higher.
        
        ![image](https://user-images.githubusercontent.com/108616378/219864093-ea34f122-f36f-4734-a538-ca79cd1f552e.png)

    9. Inside the **`Macro Recorder`**, look for the `POST` **request** where you logged in as `Ed` as well as the following GET request. Highlight both of those requests within the `Macro Recorder` window and click `OK` .
        
        ![image](https://user-images.githubusercontent.com/108616378/219864109-1396f51e-09bd-47cd-9084-459a17644989.png)
        
    10. Those two highlighted requests in the previous dialog box now appear inside the Macro Editor window. Give the macro a description, such as `LogInMacro`, as follows:
    11. Click the Configure item button to validate that the `username` and `password` values are **correct**. Click **OK** when done.
    12. Click **OK** to close the **Macro Editor**. You should see the **newly-created** macro in the **Session handling** action editor. **Click OK** to close this **dialog window.**
    13. After closing the **Session handling action** **editor**, you are returned to the Session handling rule editor where you now see the Rule Actions section populated with the name of your macro. Click the Scope tab of this window to define which tool will use this rule.
        
        ![image](https://user-images.githubusercontent.com/108616378/219864118-d7353d77-d9a2-453a-bc99-3a3479bbc6b0.png)
        
    14. On the Scope tab of the **Session handling rule editor**, uncheck the other boxes, leaving only the **Repeater checked**. Under **URL Scope**, click the Include **all URLs** **radio** button. Click **OK** to close this editor, as follows:
        
       ![image](https://user-images.githubusercontent.com/108616378/219864130-4858171a-ad53-4777-ba29-c28f34ec8b91.png)
        
    15. You should now see the new session-handling rule listed in the **Session Handling Rules** window.
    16. Return to the Repeater tab where you, previously, were not logged in to the application. Click the Go button to reveal that you are now logged in as Ed! This means your session-handling rule and associated macro worked:
- **Getting caught in the cookie jar**
    
    ### How it works...
    
    The Burp `Cookie Jar` is used by `session-handling rules` for `cookie-handling` when automating requests against a target application. In this recipe, we looked into the Cookie Jar, understood its contents, and even modified one of the values of a captured cookie. Any subsequent session-handling rules that use the default Burp Cookie Jar will see the modified value in the request.
    
- **Adding great `pentester` plugins**
    1. Switch to the Burp Extender tab. Go to the BApp Store and find two
    **plugins—Retire.js** and **Software Vulnerability Scanner**. Click the Install button for each plugin, as follows:
    2. After installing the two plugins, go to the Extender tab, then Extensions, and then the Burp Extensions section. Make sure both plugins are enabled with check marks inside the check boxes. Also, notice the Software Vulnerability Scanner has a new tab.
    3. Return to the Firefox browser and browse to the Mutillidae homepage. Perform a lightweight, less-invasive passive scan by right-clicking and selecting Passively scan this branch, as follows:
        
        []()
        
    4. Note the additional findings created from the two plugins. The `Vulners` plugin, which is the Software Vulnerability Scanner, found numerous `CVE issues`, and `Retire.js` identified five instances of a vulnerable version of `jQuery`.
- **Creating new issues via the Manual-Scan Issues Extension**
    1. Switch to the Burp Extender tab. Go to the BApp Store and find the plugin
    labeled Manual Scan Issues. Click the Install button:
    2. Return to the Firefox browser and browse to the Mutillidae homepage.
    3. Switch to the Burp Proxy | HTTP history tab and find the request you just made browsing to the homepage. Click the Response tab. Note the overly verbose Server header indicating the web server type and version along
    with the operating system and programming language used. This information can be used by an attacker to fingerprint the technology stack and identify vulnerabilities that can be exploited:
    4. Since this is a finding, we need to create a new issue manually to capture it for our report. While viewing the Request, right-click and select Add Issue
    5. A pop-up dialog box appears. Within the General tab, we can create a new issue name of Information Leakage in Server Response. Obviously, you may add more verbiage around the issue detail, background, and remediation areas, as follows:
        
       ![image](https://user-images.githubusercontent.com/108616378/219864173-91c4a706-a3a2-489c-8888-42e40df59c54.png)
        
    6. If we flip to the HTTP Request tab, we can copy and paste into the text area the contents of the Request tab found within the message editor
    7. If we flip to the HTTP Response tab, we can copy and paste into the text area the contents of the Response tab found within the message editor.
    8. Once completed, flip back to the General tab and click the Import Finding button. You should see the newly-created scan issue added to the Issues window, as follows:
    
    ### How it works...
    
    In cases where an issue is not available within the Burp core issue list, a tester
    can create their own issue using the Manual-Scan Issue Extension. In this recipe,
    we created an issue for Information Leakage in Server Responses.
    
- **Working with the `Active Scan++` Extension**
    
    Some extensions assist in finding vulnerabilities with specific payloads, such as XML, or help to find hidden issues, such as cache poisoning and DNS rebinding. In this recipe, we will add an active scanner extension called Active Scan++, which assists with identifying these more specialized vulnerabilities.
    Note: This plugin requires the Burp Professional edition.
    
    ### How to do it...
    
    1. Switch to the Burp `**Extender | BApp** Store` and select the `Active Scan++` extension. Click the Install button to install the extension, as follows:
    2. Return to the Firefox browser and browse to the `Mutillidae` homepage.
    3. Switch to the Burp Target tab, then the Site map tab, right-click on the `mutillidae` folder, and select Actively scan this branch, as follows:
        
        ![image](https://user-images.githubusercontent.com/108616378/219864194-e4b75926-c78b-46bc-a296-31d2146918ef.png)
        
    4. When the Active scanning wizard appears, you may leave the default settings and click the Next button
    Follow the prompts and click OK to begin the scanning process.
    5. After the active scanner completes, browse to the Issues window. Make note of any additional issues found by the newly-added extension. You can always tell which ones the extension found by looking for the This issue
    was generated by the Burp extension: `Active Scan++` **message**, as follows:
    
    ### How it works...
    
    Burp functionality can be extended beyond core findings with the use of extensions. In this recipe, we installed a plugin that extends the Active Scanner functionality to assist with identifying additional issues such as Arbitrary Header Injection, as seen in this recipe
