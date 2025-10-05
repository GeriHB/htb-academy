## Challenge

The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt

## Solution

First I tried the path traversal by making the link form:

`http://94.237.54.42:32961/index.php?language=languages/es.php` to

`http://94.237.54.42:32961/index.php?language=../../../../../etc/passwd`

And it showed, `Illegal path specified!` which gives me an idea that the initial folder should be `languages`.

So, I try the `http://94.237.54.42:32961/index.php?language=languages/../../../../../../etc/passwd/` but still nothing got back. Now, not even an error.

This makes me think that the first protection was bypassed, but now the file is not found. And this means that there is another protection, like removing `../`.

In this case, I try double them, so when they are removed, if not done recursively I will be able to read the file.

And I send a request to:

```http
GET /index.php?language=languages/....//....//....//....//....//....//etc/passwd HTTP/1.1
```

In this case I got the `etc/passwd` file, which means I bypassed all the protection filters here for path traversal.

So let's just modify it to read the `/flag.txt` file

```http
GET /index.php?language=languages/....//....//....//....//....//....//flag.txt HTTP/1.1
```

And I got the content: `HTB{64$!c_f!lt3r$_w0nt_$t0p_lf!}`

