## CVE's Shodan Dorks.

* Big IP shodan Search:- 

`http.title:"BIG-IP&reg;-Redirect" org:Org`

* CVE 2020-3452
 
` http.html_hash:-628873716 
“set-cookie: webvpn;”` 

* CVE CVE-2019-11510

`http.html:/dana-na/`  

* CVE-2020–5902
 
 ```inurl:/tmui/login.jsp```

Databases
Shodan Database Results
Databases often hold critical bits of information. When exposed to the public internet—whether for ease of development access or simply due to misconfiguration—can open up a huge security hole.

To find MongoDB database servers which have open authentication over the public internet within Shodan, the following search query can be used:

"MongoDB Server Information" port:27017 -authentication

MongoDB also has a web management application similar to phpMyAdmin called Mongo Express Web GUI, which we can find with the following query:

"Set-Cookie: mongo-express=" "200 OK"

Similarly, to find MySQL-powered databases:

mysql port:"3306"

To lookup popular ElasticSearch-powered instances:

port:"9200" all:"elastic indices"

And to look up PostgreSQL databases:

port:5432 PostgreSQL

¶Exposed ports
Shodan Exposed Ports
Searching for services running on open ports accessible on the public internet—like FTP servers, SSH servers and others—is possible by using the following queries.

For FTP, querying for proftpd, a popular FTP server:

proftpd port:21

To look for FTP servers that allow anonymous logins:

"220" "230 Login successful." port:21

To query for OpenSSH, a popular SSH server:

openssh port:22

For Telnet, querying for port 23:

port:"23"

To look up EXIM-powered mail servers on port 25:

port:"25" product:"exim"

Memcached, commonly seen on port 11211, has been a major source of UDP amplification attacks leading to huge DDoS attacks. Services running Memcached available on the public internet are often exploited for these attacks:

port:"11211" product:"Memcached"

Jenkins is a popular automated build, deploy and test tool, often the starting point of any software being built for release. It can be found via the following query:

"X-Jenkins" "Set-Cookie: JSESSIONID" http.title:"Dashboard"

¶DNS servers
Shodan Dns Servers Dork
DNS servers with recursion enabled can be a huge source of network threats. To find these servers, one can use the query:

"port: 53" Recursion: Enabled

¶Network infrastructure
Shodan Network Infrastructure Dork
To find devices running a specific version of a RouterOS operating system that powers routers, switches and other networking equipment from the company MikroTik, we use the following search query:

port:8291 os:"MikroTik RouterOS 6.45.9"

This allows us to find those switches, routers and other networking gear running an older and possibly vulnerable version of the RouterOS operating system which runs on port number 8291, used for the web management UI.

¶Web servers
Shodan Web Server Dork
Shodan makes it possible to find and filter out web server versions as well. For example, we'll use this to find IPs that host a specific version of the popular web server Apache:

product:"Apache httpd" port:"80"

With the above query, we can find Apache web servers on port 80, the most common port for web servers.

Similarly, to look up Microsoft IIS-powered websites and web servers:

product:"Microsoft IIS httpd"

To look up Nginx-powered websites and web servers:

product:"nginx"

The above product query can be combined with the "port" option too. For example, if you wish to lookup Nginx-powered web servers on port 8080:

"port: 8080" product:"nginx"

¶Operating systems
Shodan Operating Systems Dork
Querying for older and possibly end-of-life operating systems like Windows 7 is also possible in Shodan, by using the following query:

os:"windows 7"

Similarly, to look up specific build versions of Windows 10, the following query can be used, wherein we look up Windows 10 Home edition with build version 19041:

os:"Windows 10 Home 19041"

To filter and find Linux-based devices, the following query can be used:

os:"Linux"

¶Filtering by country, city or location
Shodan Location Filters Dork
At certain points of time, the amount of data returned by Shodan might be a bit too much. To make things easier to filter, the Country or City filter can be applied:

For example, if you wish to filter by country:

country:"UK"

To filter by city:

"city: London"

Last but not least, you can even look up via GPS coordinates of a region or city:

geo:"51.5074, 0.1278"

This location filter can be combined with other filters as well. For example, if you wish to find Windows 7 devices in the United Kingdom, the following query can be used:

os:"windows 7" country:"UK"

¶SSL certificates
Shodan SSL Certifications Dork
Another advantage of Shodan is that it can be used to find SSL certificates that are expired or self-signed.

To find self-signed certificates, the following query can be used:

ssl.cert.issuer.cn:example.com ssl.cert.subject.cn:example.com

To find expired SSL certificates, this query can be used:

ssl.cert.expired:true
