# #**What is CRLF injection?**

*CRLF injection* is a vulnerability that lets a malicious hacker inject carriage return (CR) and linefeed (LF) characters to change the way a web application works or to confuse its administrator. There are two main malicious uses for CRLF injections: *log poisoning* (also called *log injection, log splitting,* or *log forging)* and *HTTP response splitting*.

# #****What is log poisoning?****

In a log poisoning attack based on CRLF injection, a malicious hacker injects CRLF characters into web server log files to confuse both automatic log analysis systems and system administrators browsing the logs manually.

# #****The format of web server logs****

```
host ident user date request status size
```

For example:

```
233.252.0.123 - - [11/Oct/2022:11:34:50 +0100] "GET /example.php?id=3 HTTP/1.0" 200 452
```

Here is how you would read this entry:

- `233.252.0.123` is the host – the IP address from which the request came.
- `-` is the **[RFC 1413](https://www.rfc-editor.org/rfc/rfc1413)** identity of the client. The dash (-) means no data, which is the usual value.-
- `-` is the user ID of the person requesting the document. The dash (-) means no data, which is the usual value (unless there is authentication in *.htaccess*).
- `[11/Oct/2022:11:34:50 +0100]` is the timestamp of when the request was received, usually in the **[strftime](https://man7.org/linux/man-pages/man3/strftime.3.html)** format: *%d/%b/%Y:%H:%M:%S %z*.
- `"GET /example.php?id=3 HTTP/1.0"` is the request line received from the client and includes the HTTP method (`GET`), the requested resource and parameters (`/example.php?id=3`), and the protocol (`HTTP/1.0`).
- `200` is the **[HTTP status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)** sent to the client.
- `452` is the size of the returned object in bytes.

Another standard format is the Combined Log Format, which is similar but with a few extra fields.

# #****Example of log poisoning****

Imagine that the client is able to inject CR and LF characters into requests sent to the *www.example.com* web server, and it sends the following request:

```
https://www.example.com/example.php?id=3+HTTP%2F1.0%22+200+452%0D%0A
10.0.23.30+-+admin+%5B01%2FJan%2F2020%3A00%3A00%3A00+%2B0100%5D+%22GET+%2Fadmin.php%3Fuserid%3D12
```

The request contains a fake log entry, so when it is logged, the log file will include an extra line:

```
233.252.0.123 - - [11/Oct/2022:11:34:50 +0100] "GET /example.php?id=3 HTTP/1.0" 200 452
10.0.23.30 - admin [01/Jan/2020:00:00:00 +0100] "GET /admin.php?userid=123 HTTP/1.0" 200 452
```

The underlined characters represent content that was injected using CRLF injection (`%0D%0A` are encoded CRLF characters).

# #****What is HTTP response splitting?****

In an HTTP response splitting attack, the attacker injects CRLF sequences into an HTTP response to modify the way the browser interprets HTTP headers and the request body. CRLF injections can be used to add malicious content to the request body or to add extra HTTP headers

****HTTP response splitting with XSS****

The attacker sends the following payload in a phishing email that encourages the user to click a link or button:

```
http://www.example.com/example.php?id=%0d%0aContent-Length:%200%0d%0a%0d%0a
HTTP/1.1%20200%20OK%0d%0aContent-Type:%20text/html%0d%0aContent-Length:%2025%0d%0a%0d%0a
%3Cscript%3Ealert(1)%3C/script%3E
```

The payload uses CRLF injection to split the HTTP response as follows:

- `http://www.example.com/example.php?id=` – starting a valid request to a page with a CRLF injection vulnerability.
- `%0d%0aContent-Length:%200` – a fake HTTP response header of `Content-Length: 0`. This causes the web browser to treat this response as terminated and start parsing the next response.
- `%0d%0a%0d%0aHTTP/1.1%20200%20OK` – the injected new response begins here with a double CRLF sequence followed by `HTTP/1.1 200 OK`.
- `%0d%0aContent-Type:%20text/html` – another fake HTTP response header: `Content-Type: text/html`. This is required for the browser to treat this data as HTML content.
- `%0d%0aContent-Length:%2025` – yet another fake HTTP response header: `Content-Length: 25`. This instructs the browser to parse only the next 25 bytes and discard any remaining data as junk, causing it to ignore the legitimate HTTP content sent by the web server.
- `%0d%0a%0d%0a%3Cscript%3Ealert(1)%3C/script%3E` – a double CRLF sequence signals that the headers are over and the response body starts. The injected page content is `<script>alert(1)</script>`, which causes the user’s browser to display an alert instead of the actual *example.php* page.
