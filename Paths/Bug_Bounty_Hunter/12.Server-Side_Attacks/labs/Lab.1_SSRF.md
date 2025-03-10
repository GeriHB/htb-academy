## Challenge 

Exploit a SSRF vulnerability to identify an internal web application. Access the internal application to obtain the flag. 

## Solution

After checking for the availability of a date, I send the request to Burp Repeater, and I see the body of the `POST` request is:

```http
dateserver=http://dateserver.htb/availability.php&date=2024-01-16
```

