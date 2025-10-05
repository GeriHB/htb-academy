## Challenge

Use Burp Intruder to fuzz for '.html' files under the /admin directory, to find a file containing the flag.

## Solution

I send a request to the `94.237.56.27:49004/admin` to the Burp Intruder.

There I selected the `Snipper Attack` and added the possition like this: `GET /admin/TEST HTTP/1.1`.

I used the `Simple list` payload type, and took pasted the `/usr/share/seclists/Discovery/Web-Content/common.txt` wordlist in the payload configuration.

In the payload processing I added a rule to add a suffix `.html`, and started the attack.

After a while I saw a `200 OK` on the `2010.html` file, and after seeing its response I saw the flag which was: `HTB{burp_1n7rud3r_fuzz3r!}`