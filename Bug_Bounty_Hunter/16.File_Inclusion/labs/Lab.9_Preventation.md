## Challenge #1
What is the full path to the php.ini file for Apache? 

## Solution 1

I use the `find` tool:

`find / -name "php.ini 2>/dev/null`

and the result is `/etc/php/7.4/apache2/php.ini`.

## Challenge #2

 Edit the php.ini file to block system(), then try to execute PHP Code that uses system. Read the /var/log/apache2/error.log file and fill in the blank: system() has been disabled for ________ reasons. 

 ## Solution #2

 I edited the `php.ini` file and added in the end: `disable_functions = system`, then restarted Apache:

 ```sh
 sudo systemctl restart apache2
 ```

 I created a simple php script, and saved it to `whoami.php`:

 ```php
 <?
 system("whoami");
 ?>
 ```

and saved it to the web server directory `/var/www/html`.

In a browser I tried to access it by going to `http://10.129.29.112/whoami.php`, and then checked the error logs:

`sudo tail -n 20 /var/log/apache2/error.log`

The result was `system() has been disabled for security reasons in /var/www/html/whoami.php on line 2`, and the lab is solved.

