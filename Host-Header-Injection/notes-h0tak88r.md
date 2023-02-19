**Here we have Simple Request Which We have captured Through burp,**

```python
GET / HTTP/1.1
**Host: securiumsolutions.com**
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
```

To Check Here We need To **change Host: section** To Your controlled Website And Check it response if you got **200 302** Just Check Response Section If **your Controlled Host Is reflected** That mean it vulnerable For **Host Header Injection** ,

If This Now Wor then add **X-Forwarded-Host: youraddress.com** we can also Host Section To **X-Forwarded-Host**: section As eg:

```python
GET / HTTP/1.1
**Host: yoursite.com
X-Forwarded-Host: securiumsolutions.com**
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
```

**Alright,** As We see How Web application Vulnerable For Host Header Injection Now , How it allow an bad actor To misuse and takeover any user account by **Host Header Poisoning** ,

**Where We need to look ?**

- *Reset password Functionality*
- *Signup*
- *Confirmation Token*

### **Testing Steps:**

**Step1:** Supposed We have Confirmation Page Which Allow users Only after Verification Of Email which sent to user Email right , But here An attacker can Confirm Victim account without Authenticate Victim Email due to Host Headed poisoning Which leak confirmation Token With Attacker Controlled Server.

**Here,** We have Our target As **xyz.com** Now we are Creating New account On xyz.com And here In Email which need To verify here To test Simply add Victim email [victimmail@gmail.com](mailto:victimmail@gmail.com) and Fillup Request And capture Request Using Burp , Before Process Make the functionality Is running under **xyz.com** or using third party services ,

Now We captured Request On repeater section Now , here simply change **Host** section , or Add **X-Forwarded-Host:** that we Discuss Already And Look Your Inbox how it route .

**Here** We have Different Functionality Which Functionality Running Under Some third party services , Look For Below Screenshot,

![https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_601,h_227/https://securiumsolutions.com/blog/wp-content/uploads/2020/08/image-12.png](https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_601,h_227/https://securiumsolutions.com/blog/wp-content/uploads/2020/08/image-12.png)

**As** above picture We see Functionality Running Under third party service but it carries Original Host As Parameter “domain” Here Simply Change Domain To Your Controlled Domain , Make sure you have php, SimplePython, or ngrok server that You can capture Leak token If Target Is vulnerable,

**Now** User When victim click Confirmation Link On His Email At the time he/she click And Attacker Will Get Victim **Confirmation Token** Inside Attacker Server which Leak Due to Host Header Poisoning,

![https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_630,h_123/https://securiumsolutions.com/blog/wp-content/uploads/2020/08/image-13.png](https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_630,h_123/https://securiumsolutions.com/blog/wp-content/uploads/2020/08/image-13.png)

**As** Above Pic Now Attacker Can takeover Victim Account Due To Host Header Is not Implement This allow A malicious attacker control of a particular individual’s Account In different scenario .

### **How To Mitigate This Type Of Issue :**

*· Validate the headers that supplied into the requests Which You Must Need to configure Properly That an bad actor can’t control.*

*· Also use multi-factor authentication to prevent account hijacking , and one such method is SMS Authentication.*
