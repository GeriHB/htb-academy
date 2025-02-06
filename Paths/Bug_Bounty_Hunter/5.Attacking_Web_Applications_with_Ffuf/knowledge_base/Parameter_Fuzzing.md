# Parameter Fuzzing 

## Parameter Fuzzing - GET

When we do a recursive ffuf at `admin.academy.htb` there is one link: `http://admin.academy.htb:50837/admin/admin.php`, in which where we go it says:

`You don't have access to read the flag!`

So this means there is something authenticating us, if we have access or not.

Since, we don't have credentials, or any cookies to be verified, maybe there is a key that can be passed to the page to allow us access.

These keys are usually passed as a parameter with either a `GET` or a `POST` method.

```sh
ffuf -w WORDLIST:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key
```

Since I'm using `fish` I need to encapsulate the url in quotes:

```sh
ffuf -w WORDLIST:FUZZ -u "http://admin.academy.htb:PORT/admin/admin.php?FUZZ"
```

This gives a lot of `200 OK` as expected, but they have a common file size of 798, so we filter that out.

```sh
ffuf -w WORDLIST:FUZZ -u "http://admin.academy.htb:PORT/admin/admin.php?FUZZ" -fs 798
```

And we got a result:

```sh
user                    [Status: 200, Size: 783, Words: 221, Lines: 54, Duration: 32ms]
```

## Parameter Fuzzing - POST

`POST` requests are not passed with the URL but in the data field within the request.

To fuzz the data field we use the `-d` flag and the `-X POST` to send a `POST` request:

```sh
ffuf -w WORDLIST:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

In our case, I ran:

```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:50837/admin/admin.php -X POST -d 'FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs 798
```

and the result was:

```sh
id                      [Status: 200, Size: 768, Words: 219, Lines: 54, Duration: 30ms]
user                    [Status: 200, Size: 783, Words: 221, Lines: 54, Duration: 29ms]
```

Now when I send a `curl` to an `id=1`:

```sh
curl http://admin.academy.htb:50837/admin/admin.php -X POST -d 'id=1' -H 'Content-Type: application/x-www-form-urlencoded'
```

I got a response saying `Invalid id!`.

## Value Fuzzing

Some times we don't find a suitable wordlist, and we have to create one by ourselves, to match the values that we will be brute-forcing.

For example, in our case, we have an `id` and we want to try numbers from 1 to 1000.

I did this via a python script:

```py
# Python script to generate a wordlist with numbers from 1 to 1000

# Define the output file path
output_file = 'wordlist.txt'

# Open the file in write mode
with open(output_file, 'w') as file:
    # Write numbers from 1 to 1000 into the file
    for i in range(1, 1001):
        file.write(f"{i}\n")

print(f"Wordlist saved to {output_file}")
```
 