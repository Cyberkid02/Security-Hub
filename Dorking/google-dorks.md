# Google dorks helper
https://dorks.faisalahmed.me/

# Find Bug Bounty programs
"submit vulnerability report" | "powered by bugcrowd" | "powered by hackerone" -inurl:news -site:*.de
# Code share sites
site:http://ideone.com | site:http://codebeautify.org | site:http://codeshare.io | site:http://codepen.io | site:http://repl.it | site:http://jsfiddle.net "company"
# GitLab/GitHub/Bitbucket
site:github.com | site:gitlab.com | site:bitbucket.org "company"
# Stackoverflow
site:stackoverflow.com "target.com"
# Project management sites
site:http://trello.com | site:*.atlassian.net "company"
# Pastebin-like sites
site:http://justpaste.it | site:http://pastebin.com "company"
# Config files
site:target.com ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:env | ext:ini
# Database files
site:target.com ext:sql | ext:dbf | ext:mdb
# Backup files
site:target.com ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup
# .git folder
inurl:"/.git" target.com -github
# Exposed documents
site:target.com ext:doc | ext:docx | ext:odt | ext:pdf | ext:rtf | ext:sxw | ext:psw | ext:ppt | ext:pptx | ext:pps | ext:csv
# Other files
site:target.com intitle:index.of | ext:log | ext:php intitle:phpinfo "published by the PHP Group" | inurl:shell | inurl:backdoor | inurl:wso | inurl:cmd | shadow | passwd | boot.ini | inurl:backdoor | inurl:readme | inurl:license | inurl:install | inurl:setup | inurl:config | inurl:"/phpinfo.php" | inurl:".htaccess" | ext:swf
# SQL errors
site:target.com intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"
# PHP errors
site:target.com "PHP Parse error" | "PHP Warning" | "PHP Error"
# Login pages
site:target.com inurl:signup | inurl:register | intitle:Signup
# Open redirects
site:target.com inurl:redir | inurl:url | inurl:redirect | inurl:return | inurl:src=http | inurl:r=http
# Apache Struts RCE
site:target.com ext:action | ext:struts | ext:do
# Search in pastebin
site:pastebin.com target.com
# Linkedin employees
site:linkedin.com employees target.com
# Wordpress files
site:target.com inurl:wp-content | inurl:wp-includes
# Subdomains
site:*.target.com
# Sub-subdomains
site:*.*.target.com
#Find S3 Buckets
site:.s3.amazonaws.com | site:http://storage.googleapis.com | site:http://amazonaws.com "target"
# Traefik
intitle:traefik inurl:8080/dashboard "target"
# Jenkins
intitle:"Dashboard [Jenkins]"

# Other 3rd parties sites
https://www.google.com/search?q=site%3Agitter.im%20%7C%20site%3Apapaly.com%20%7C%20site%3Aproductforums.google.com%20%7C%20site%3Acoggle.it%20%7C%20site%3Areplt.it%20%7C%20site%3Aycombinator.com%20%7C%20site%3Alibraries.io%20%7C%20site%3Anpm.runkit.com%20%7C%20site%3Anpmjs.com%20%7C%20site%3Ascribd.com%20%22united%22
# Backup files
https://www.google.com/search?q=site%3Aunited.com%20ext%3Abkf%20%7C%20ext%3Abkp%20%7C%20ext%3Abak%20%7C%20ext%3Aold%20%7C%20ext%3Abackup
# Login pages
https://www.google.com/search?q=site%3Aunited.com%20inurl%3Asignup%20%7C%20inurl%3Aregister%20%7C%20intitle%3ASignup
# Config files
https://www.google.com/search?q=site%3Aunited.com%20ext%3Axml%20%7C%20ext%3Aconf%20%7C%20ext%3Acnf%20%7C%20ext%3Areg%20%7C%20ext%3Ainf%20%7C%20ext%3Ardp%20%7C%20ext%3Acfg%20%7C%20ext%3Atxt%20%7C%20ext%3Aora%20%7C%20ext%3Aenv%20%7C%20ext%3Aini
# .git folder
https://www.google.com/search?q=inurl%3A%5C%22%2F.git%5C%22%20united.com%20-github
# Database files
https://www.google.com/search?q=site%3Aunited.com%20ext%3Asql%20%7C%20ext%3Adbf%20%7C%20ext%3Amdb
# Open redirects
https://www.google.com/search?q=site%3Aunited.com%20inurl%3Aredir%20%7C%20inurl%3Aurl%20%7C%20inurl%3Aredirect%20%7C%20inurl%3Areturn%20%7C%20inurl%3Asrc%3Dhttp%20%7C%20inurl%3Ar%3Dhttp
# Code share sites
https://www.google.com/search?q=site%3Asharecode.io%20%7C%20site%3Acontrolc.com%20%7C%20site%3Acodepad.co%20%7Csite%3Aideone.com%20%7C%20site%3Acodebeautify.org%20%7C%20site%3Ajsdelivr.com%20%7C%20site%3Acodeshare.io%20%7C%20site%3Acodepen.io%20%7C%20site%3Arepl.it%20%7C%20site%3Ajsfiddle.net%20%22united%22
# Pastebin-like sites
https://www.google.com/search?q=site%3Ajustpaste.it%20%7C%20site%3Aheypasteit.com%20%7C%20site%3Apastebin.com%20%22united%22
# Linkedin employees
https://www.google.com/search?q=site%3Alinkedin.com%20employees%20united.com
# Project management sites
https://www.google.com/search?q=site%3Atrello.com%20%7C%20site%3A*.atlassian.net%20%22united%22
# Other files
https://www.google.com/search?q=site%3Aunited.com%20intitle%3Aindex.of%20%7C%20ext%3Alog%20%7C%20ext%3Aphp%20intitle%3Aphpinfo%20%5C%22published%20by%20the%20PHP%20Group%5C%22%20%7C%20inurl%3Ashell%20%7C%20inurl%3Abackdoor%20%7C%20inurl%3Awso%20%7C%20inurl%3Acmd%20%7C%20shadow%20%7C%20passwd%20%7C%20boot.ini%20%7C%20inurl%3Abackdoor%20%7C%20inurl%3Areadme%20%7C%20inurl%3Alicense%20%7C%20inurl%3Ainstall%20%7C%20inurl%3Asetup%20%7C%20inurl%3Aconfig%20%7C%20inurl%3A%5C%22%2Fphpinfo.php%5C%22%20%7C%20inurl%3A%5C%22.htaccess%5C%22%20%7C%20ext%3Aswf
# Sub-subdomains
https://www.google.com/search?q=site%3A*.*.united.com
# Jenkins
https://www.google.com/search?q=intitle%3A%5C%22Dashboard%20%5BJenkins%5D%5C%22%20%22united%22
# Traefik
https://www.google.com/search?q=intitle%3Atraefik%20inurl%3A8080%2Fdashboard%20%22united%22
# Cloud buckets S3/GCP
https://www.google.com/search?q=site%3A.s3.amazonaws.com%20%7C%20site%3Astorage.googleapis.com%20%7C%20site%3Aamazonaws.com%20%22united%22
# SQL errors
https://www.google.com/search?q=site%3Aunited.com%20intext%3A%5C%22sql%20syntax%20near%5C%22%20%7C%20intext%3A%5C%22syntax%20error%20has%20occurred%5C%22%20%7C%20intext%3A%5C%22incorrect%20syntax%20near%5C%22%20%7C%20intext%3A%5C%22unexpected%20end%20of%20SQL%20command%5C%22%20%7C%20intext%3A%5C%22Warning%3A%20mysql_connect()%5C%22%20%7C%20intext%3A%5C%22Warning%3A%20mysql_query()%5C%22%20%7C%20intext%3A%5C%22Warning%3A%20pg_connect()%5C%22
# Exposed documents
https://www.google.com/search?q=site%3Aunited.com%20ext%3Adoc%20%7C%20ext%3Adocx%20%7C%20ext%3Aodt%20%7C%20ext%3Apdf%20%7C%20ext%3Artf%20%7C%20ext%3Asxw%20%7C%20ext%3Apsw%20%7C%20ext%3Appt%20%7C%20ext%3Apptx%20%7C%20ext%3Apps%20%7C%20ext%3Acsv
# Wordpress files
https://www.google.com/search?q=site%3Aunited.com%20inurl%3Awp-content%20%7C%20inurl%3Awp-includes
# Apache Struts RCE
https://www.google.com/search?q=site%3Aunited.com%20ext%3Aaction%20%7C%20ext%3Astruts%20%7C%20ext%3Ado
# GitLab/GitHub/Bitbucket
https://www.google.com/search?q=site%3Agithub.com%20%7C%20site%3Agitlab.com%20%7C%20site%3Abitbucket.org%20%22united%22
# Subdomains
https://www.google.com/search?q=site%3A*.united.com
# Stackoverflow
https://www.google.com/search?q=site%3Astackoverflow.com%20%22united.com%22
# PHP errors
https://www.google.com/search?q=site%3Aunited.com%20%5C%22PHP%20Parse%20error%5C%22%20%7C%20%5C%22PHP%20Warning%5C%22%20%7C%20%5C%22PHP%20Error%5C%22
