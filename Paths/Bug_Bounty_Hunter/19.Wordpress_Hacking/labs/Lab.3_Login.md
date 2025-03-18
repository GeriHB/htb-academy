
## Challenge 1

Search for "WordPress xmlrpc attacks" and find out how to use it to execute all method calls. Enter the number of possible method calls of your target as the answer.

## Solution 1

When we visit the page on `/xmlrpc.php` it says that only `POST` methods are allowed, so we send a `POST` request with the following method, to list all of the methods.

```http
POST /xmlrpc.php HTTP/1.1
Host: 94.237.59.30:58652
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 85

<methodCall><methodName>system.listMethods</methodName><params></params></methodCall>
```

And the results shows us all of the methods, which in total are 80.