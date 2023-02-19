# ****Business logic vulnerabilities****

- ****What is logic ?****
    
    **Our logic is always flawed it’s as simple as that.** 
    
    ## [+] **In Reak Life Example**
    
    # **Tought experiment**
    
    What I have in front of me:
    
    - 1 knife
    - 1 slice of bread
    - 1 Jar Nutella
    
    When I ask the people for instructions on how to make a **sandwich**, a few things will get called forward. Before you read on, i want you to think about how you would tell me to make a sandwich.
    
    I would tell a person of average intelligence to make a **sandwich** by putting the **knife** in the **jar** and smearing it on the bread **but** now i want you to imagine explaining this to a person **who never held a knife before.** Who **never opened a jar** before. Who **never even heard of a sandwich** before. This is why logic issues arise.
    
    ## [+] In Softwares
    
    We are often using software of which we do have some basic idea’s so in a way you can compare us users as caveman with very basic knowledge of tool usage, but suddenly **they gave you a table-saw**. Now you have made stone tools yourself and you do know how to use those but this magical thing you’ve never seen before, that’s new to you.
    
    It’s the exact same thing with software, we are using this software and **even regular users will have trouble and cause unintended behaviour**. This behaviour may not always lead to a flaw and definitely not always to a security flaw but it **does occur quite often**. Much more often than people think.
    
- ****What is a business logic vulnerability****
    
    In recent times, everything needs to go faster and faster with the coming up of agile development, We humans suck at predicting the consequences of our actions and even if we could foresee all of our actions, we can’t take into account what others will do.
    
    **[+] there are a couple of ways a business logic vulnerability can sneak in:**
    
    - The developers and analysts often don’t know how the old code works anymore**.** This is an excellent opportunity for logic issues! Lacking knowledge of existing features might make it so that some properties of that features have been overlooked, this creates a great entry point for the juicy logic vulnerabilities we are looking for.
    - The lack of documentation also means that when teams have to work together or create an integration, its really hard for them to communicate effectively and this may in turn lead to missed properties of the feature.
    - The logic is so complex the people working on it get confused themselves, allowing them to make mistakes
    - A bad process is in place, allowing these types of issues to slip through several layers of testing
    
- **Examples**
    - We have a feature to pay for an order but it’s not documented propperly.
    - The developers have to edit the system after a year to include mobile banking but since they don’t have documentation, they **miss a piece of code**. It’s hidden deep inside the existing code but it **allows you to enter 999999999 in the credit card field** and it will automatically pay for you throughout the application.
    - This has been done by a developer in the original code but has been put in a method that should normally never be reached, he did this for testing purposes.
    - With the update of the login system, the developers had no idea what this code did due to it’s complexity and they put it the payment procedure, introducing a logic flaw because **anyone can now pay by entering 999999999 in the credit card field**.
    - When you click a link “**forgot password**” there’s basically two systems that will work together.
        - **Reset password generation link**
        - **Mail sending link**
    - If we enter the following payload
    
    ```
    POST /resetPass.php
    {
    "email":"victim@mail.com",
    "email":"attacker@mail.com"
    }
    ```
    
    - We can trigger the server that generates the link to **generate a link for [victim@mail.com](mailto:victim@mail.com)** but the mail server **might only check the last “email” parameter it sees and send it off to [attacker@mail.com](mailto:attacker@mail.com)**
    - Imagine you are a triager at a bug bounty platform, but your system has gotten very complex to triage bugs and you might accidentally have part of your **admin panel exposed to the public** without even knowing it because **the developers forgot to add authentication to that page**.
    - There might not be any code reviews going on, which allows for the following statement. The OR should be an AND here:
    
    ```
    SELECT * FROM users WHERE username='$username' OR password='$password'
    ```
    
- ****Properties of a logic vulnerability****
    - You can’t detect them with automatic process efficiently. It’s really hard to teach a robot logic. 🙂
    - These issues are inherently hard to detect manually since they already slipped passed the analyst, developer and tester who could have all caught it.
    - They range in severity from low to critical in severity depending on the functionality and how it’s impacted
- ****Development process****
    - ****Waterfal model****
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57693a3a-9bdc-4bc9-b6f2-b4aac3de3c89/Untitled.png)
        
    - ****V-Model****
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65694153-8904-4045-8e1c-6dc2be300421/Untitled.png)
        
        to summarise:
        
        - Test when new releases hit production
        - Test the new features
        - Test the refactored features
        - Think about how features can interact with one another and if there might be a possible flaw in the interaction
        - Try to subscribe to any newsletters if possible
    - ****Agile****
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/77ee499f-0696-4c66-9b48-97b3efbec57c/Untitled.png)
        
        As you can see, in agile, things move in a cycle, and often a 2 week cycle. This is fast and it allows for the easy introduction of business logic flaws due to either:
        
        - Regression errors
        - A messy developer can easily have an old piece of code on the laptop and commit it, after which it gets smuggles to production in a HUGE code commit with many files. — A developer can easily make a mistake due to the time pressure or missing documentation
        - If agile is not being used properly (which is often the case), the process becomes a lot more prone to business errors
        - User stories are often not chopped up into their smallest workable tasks, which allows for missed requirements due to higher complexity — Sprints are often way too big, taking in a huge scope due to Tasks not being estimated properly, Sprint capacity being overrated, Sprints might take way too long, taking in a huge scope
        - All of these points make it easier for business logic flaws to appear and we should definitely test: New feature, Our targets every release cycle (usually 2 weeks), Refactored features
- ****What is the impact[Examples}?****
    
    The impact is highly dependant on the **specific target** and **logic flaw** that you found. It is related to the **impacted functionality** as well.
    
    - **Client side calculations** of prices in a **clothing webshop** —> **`High/Crit`**
        - This is core business for the target so **any issue related to the core business** will automatically be **more impactful**
        
        ---
        
    - When **brute forcing usernames**, you get a **200 OK status** when the username you are trying to **brute force exists** and a **403 if it does not exist** on the login page —> `**Low**`
        - This is rather low unless those usernames really have to be secret, you’d have to brute force the login names and then you have to **still guess the correct password**. This is more useful on a pentest job.
        
        ---
        
    - **Negative amounts of items** on a webshop lead to **negative prices** —> `**High/Crit**`
        - This is core business for the target so any issue related to the core business will automatically be more impactful above all, impact on money directly is very important
        
        ---
        
    - If **price = integer and amount = integer** and **total price = integer** we can overflow total price when we **price * amount** —> `**Critical**`
        - This might lead to the target returning us money which is certainly not desirable
        
        ---
        
    - Registering with the **same username as an existing user** takes over the account —> `**Critical**`
        - Account takeovers are always higher on the severity scale
        
        ---
        
    - The user manual might tell you that you can’t deactivate super admin users, but after trying it you can —> `**Medium**`
        - You have to be a privileged user to even be allowed into the **user management system** so this **lowers the severity a bit**
        
        ---
        
    - Field in the **response that’s not in the original request** but does get **processed by the server when you add it** —> `**Nothing/Critical**`
        - This really depends on the fields that is being processed here. If you can change your account type from “User” to “Admin” This would of-course be a big problem.
        
        ---
        
    - **Importing products with the same name** as existing ones **overwrites them**. Even if the products do not belong to you and you should not be able to overwrite them. —> `**Medium**`
        - You are already in a privileged position before you can import products, this lowers the severity
        
        ---
        
- ****Extra-curriculum activities****
    
    We can train our logic skills easily, there are hundreds of activities you can do to increase **your logic skills**. My favourite are **escape rooms**! This is a very fun way to increase **your logic thinking skills** and it will help you **spot those details more easily.** 
    
    A fun and free way to get started is to look up any of the **100’s of free escape rooms** you can print at home and start playing right away.
    
    [ [Free online escape games](https://escapethereview.co.uk/free-online-escape-games/#be-the-escape) ]
    
    So even though logic seems like a hard skill to train, it seems like enough people are willing to take on the challenge and have developed learning material that can help us. **It may seem dumb but yes, we can train our own logic.**
    
- **The 10 most common business logic attack vectors include:**
- Authentication flags and privilege escalations
- Critical parameter manipulation and access to unauthorized information/content
- Developer's cookie tampering and business process/logic bypass
- LDAP parameter identification and critical infrastructure access
- Business constraint exploitation
- Business flow bypass
- Exploiting clients side business routines embedded in JavaScript, Flash or Silverlight
- Identity or profile extraction
- File or unauthorized URL access & business information extraction
- Denial of Services (DoS) with business logic
