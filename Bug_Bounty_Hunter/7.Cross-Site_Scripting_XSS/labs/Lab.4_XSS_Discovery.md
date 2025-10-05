## Challenge 1

Utilize some of the techniques mentioned in this section to identify the vulnerable input parameter found in the above server. What is the name of the vulnerable parameter? 

## Solution 1

I visit the page, and it offers a form to register.

I put there some data, and click on register, I see that is a `GET` request, and copy the URL, as it has the parameters that I will test.

For the testing I use `XSStrike` tool:

```sh
python3 xsstrike.py -u "http://94.237.56.156:52003/?fullname=Test&username=HB&password=TT&email=tt%40hb.com"
```

The results show that there is one parameter which is vulnerable:

```sh
[!] Testing parameter: email
[!] Reflections found: 1
```

## Challenge 2

What type of XSS was found on the above server? "name only" 

## Solution 2

From the results it can be see that the type is `Reflected`.

