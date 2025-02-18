## Challenge 1

What is the IANA ID of the registrar of the inlanefreight.com domain? 

## Solution 1

Execute the command:

```sh
whois inlanefreight.com
```

And you see the result which is `Registrar IANA ID: 468`.

## Challenge 2

What http server software is powering the inlanefreight.htb site on the target system? Respond with the name of the software, not the version, e.g., Apache. 

## Solution 2

Add the server to the `/etc/hosts` file and execute the `curl` command:

```sh
curl -I inlanefreight.htb:51596
```

The result is `Server: nginx/1.26.1`.

## Challenge 3

What is the API key in the hidden admin directory that you have discovered on the target system? 

## Solution 3

I try to search for `robots.txt` by executing the command:

```sh
curl -i inlanefreight.htb:51596/robots.txt
```

No file found here.

Now I ran `gobuster` to discover VHosts:

```sh
gobuster vhost -u http://inlanefreight.htb:51596 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 100
```

This provided one result which is:

```sh
Found: web1337.inlanefreight.htb:51596 Status: 200 [Size: 104]
```

Now add this again to the `/etc/hosts` file.

Let's look again for the `robots.txt` by executing `curl -i web1337.inlanefreight.htb:51596/robots.txt`, which provided with the document as follows:

```http
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Tue, 18 Feb 2025 23:13:09 GMT
Content-Type: text/plain
Content-Length: 99
Last-Modified: Thu, 01 Aug 2024 09:34:59 GMT
Connection: keep-alive
ETag: "66ab56c3-63"
Accept-Ranges: bytes

User-agent: *
Allow: /index.html
Allow: /index-2.html
Allow: /index-3.html
Disallow: /admin_h1dd3n
```

So, I see a directory `/admin_h1dd3n` let's curl to that:

```sh
curl web1337.inlanefreight.htb:51596/admin_h1dd3n/
```

And it provided with the API:

```html
<!DOCTYPE html><html><head><title>web1337 admin</title></head><body><h1>Welcome to web1337 admin site</h1><h2>The admin panel is currently under maintenance, but the API is still accessible with the key e963d863ee0e82ba7080fbf558ca0d3f</h2></body></html>
```

## Challenge 4

After crawling the inlanefreight.htb domain on the target system, what is the email address you have found? Respond with the full email, e.g., mail@inlanefreight.htb. 

## Solution 4

I repeated the `gobuster` execution now with the new domain:

```sh
gobuster vhost -u http://dev.web1337.inlanefreight.htb:51596 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 100
```

and it discovered:

```sh
Found: dev.web1337.inlanefreight.htb:51596 Status: 200 [Size: 123]
```

Now I add this to the `/etc/hosts` file and use the `ReconSpider` tool on it:

```sh
python3 ReconSpider.py http://dev.web1337.inlanefreight.htb:51596
```

In the `results.json` I see the email, which is `1337testing@inlanefreight.htb`.

## Challenge 5

What is the API key the inlanefreight.htb developers will be changing too? 

## Solution 5

In the same `results.json` I see a comment which says:

`Remember to change the API key to ba988b835be4aa97d068941dc852ff33`, which is the answer to the final challenge on the module.