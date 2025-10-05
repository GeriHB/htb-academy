## Challenge 1

What's the name of the column containing "style" in it's name? (Case #1) 

## Solution

So we search for the column that contains `style` in its name:

```sh
python3 sqlmap.py -u "http://94.237.55.96:34129/case1.php?id=1" --search -C style
```

after executing this command, SQLMap asked if I want to search based on `LIKE` or the exact name.

I choose the `LIKE` option because it would search for all column names containing `style`, and it provided with the name which is `PARAMETER_STYLE`.

## Challenge 2

What's the Kimberly user's password? (Case #1) 

## Solution 2

I dumped everything via:

```sh
python3 sqlmap.py -u "http://83.136.255.180:37096/case1.php?id=1" --dump
```

It asked me if I want to try the dictionary attack on the hashes, and in the end it provided me the table with the password hashes and cracked ones.

Kimberly's password is `d642ff0feca378666a8727947482f1a4702deba0 (Enizoom1609)`.

