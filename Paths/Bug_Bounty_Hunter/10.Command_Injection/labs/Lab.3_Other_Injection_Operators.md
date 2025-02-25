## Challenge

Try using the remaining three injection operators (new-line, &, |), and see how each works and how the output differs. Which of them only shows the output of the injected command? 

## Solution

I check for a localhost ip address, and send the request to the `Burp Repeater` where I can change the body and bypass the frontend validation.

The newline `\n` doesn't work, take too long to respond.

```sh
ip=127.0.0.1\nwhoami
```

The AND operator `&` shows only the first command.

```sh
ip=127.0.0.1&whoami
```

And the pipe operator `|` shows only the output of the second command.

```sh
ip=127.0.0.1|whoami
```
