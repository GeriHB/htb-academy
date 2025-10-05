## Challenge 1
Which IP address maps to inlanefreight.com?

## Solution 1
Run the following command:

```sh
dig +short inlanefreight.com
```

and the response is `134.209.24.248` which is the answer.

## Challenge 2
Which domain is returned when querying the PTR record for 134.209.24.248?

## Solution

Run the following command:

```sh
dig -x 134.209.24.248
```

And in the results you will have:

```sh
;; ANSWER SECTION:
248.24.209.134.in-addr.arpa. 1800 IN    PTR     inlanefreight.com.
```

the answer being `inlanefreight.com`.

## Challenge 3
What is the full domain returned when you query the mail records for facebook.com?

## Solution 3
Run the command:

```sh
dig facebook.com MX
```

In the response we have the answer which is `smtpin.vvv.facebook.com`.

