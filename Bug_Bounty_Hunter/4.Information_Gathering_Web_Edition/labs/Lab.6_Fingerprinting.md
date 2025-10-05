## Challenge 1

Determine the Apache version running on app.inlanefreight.local on the target system. (Format: 0.0.0)

## Solution 1

First edit the `/etc/hosts` file and include the target and the vHosts.

Then I used `curl` to grab the banners:

```sh
curl -I app.inlanefreight.local
```

which provided with the following results:

```sh
HTTP/1.1 200 OK
Date: Tue, 18 Feb 2025 17:32:13 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: 72af8f2b24261272e581a49f5c56de40=ekdcp5h93mjsrfqepus896ukj5; path=/; HttpOnly
Permissions-Policy: interest-cohort=()
Expires: Wed, 17 Aug 2005 00:00:00 GMT
Last-Modified: Tue, 18 Feb 2025 17:32:22 GMT
Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache
Content-Type: text/html; charset=utf-8
```

so the answer is `2.4.41`.

## Challenge 2

Which CMS is used on app.inlanefreight.local on the target system? Respond with the name only, e.g., WordPress. 

## Solution 2

I used the `whatweb` tool and executed the following command:

```sh
whatweb app.inlanefreight.local
```

It provided with the following resutls, which showed that the CMS is `Joomla`:

```sh
http://app.inlanefreight.local [200 OK] Apache[2.4.41], Bootstrap, Cookies[72af8f2b24261272e581a49f5c56de40], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], HttpOnly[72af8f2b24261272e581a49f5c56de40], IP[10.129.116.214], JQuery, MetaGenerator[Joomla! - Open Source Content Management], OpenSearch[http://app.inlanefreight.local/index.php/component/search/?layout=blog&amp;id=9&amp;Itemid=101&amp;format=opensearch], Script, Title[Home], UncommonHeaders[permissions-policy]
```

## Challenge 3

On which operating system is the dev.inlanefreight.local webserver running in the target system? Respond with the name only, e.g., Debian. 

## Solution 3

This can be easily achieved, just by using the `curl` tool:

```sh
curl -I dev.inlanefreight.local
```

which provided the results that shows the OS is Ubuntu:

```sh
HTTP/1.1 200 OK
Date: Tue, 18 Feb 2025 17:38:38 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: 02a93f6429c54209e06c64b77be2180d=vb3d8v06kttbetea95ppp5bvge; path=/; HttpOnly
Expires: Wed, 17 Aug 2005 00:00:00 GMT
Last-Modified: Tue, 18 Feb 2025 17:38:47 GMT
Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache
Content-Type: text/html; charset=utf-8
```



