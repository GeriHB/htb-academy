# Basic Fuzzing

## Directory Fuzzing

`Ffuf` can be installed via:
```sh
sudo apt install ffuf
```

Main options are `-w` for wordlist and `-u` for the URL. 

Also we can assign a wordlist to a keyword, that we want to brute-force wherever it is met:

```sh
ffuf -w ./wordlist.txtLFUZZ -u http://server_ip:PORT/Halil
```

ffuf works really fast, and the speed can even be increased for example by increasing the number of threads with the option `-t` but this can cause a `DoS` or bring down your internet.

## Page Fuzzing

Now I will search for hidden pages. But for this we need to know the extension that the page is using.

For example if the server is `apache` then it uses `.php`, if it is `IIS`, then it can be `.asp, .aspx`, etc.

`Ffuf` can be used for this, and instead of adding the `FUZZ` at the directory I will add it at the extension part. Also, a wordlist for extensions should be used.

A common file to almost all websites is `index` so we will be trying that to find the extension.

```sh
ffuf -w WORDLIST -u http://server_ip:PORT/indexFUZZ
```

Now, we will see some `200 OK` for example for `.php` and in this case we will use the same wordlist for directories, but now to search for filenames:

```sh
ffuf -w WORDLIST -u http://server_ip:PORT/FUZZ.php
```

## Recursive Fuzzing

Having a number of directories with subdirectories, the methods above won't work efficiently.

To enable recursive scan, we need to add the `-recursion` flag, and also we can apply the depth with the `-recursion-depth`.

And with the `-v` flag we can see the full URLs.



