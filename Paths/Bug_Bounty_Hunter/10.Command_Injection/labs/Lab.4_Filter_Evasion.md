## Challenge

Try all other injection operators to see if any of them is not blacklisted. Which of (new-line, &, |) is not blacklisted by the web application? 

## Solution

I send the request to the Burp Repeater, and there I modify the body to check the output.

First I try the newline character:

```sh
ip=127.0.0.1%0a
```

this provided with the result of the ping command, menaing that the `\n` new-line character is not blacklisted.