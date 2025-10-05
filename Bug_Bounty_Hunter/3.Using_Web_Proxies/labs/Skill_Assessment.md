## Challenge 1

The /lucky.php page has a button that appears to be disabled. Try to enable the button, and then click it to get the flag.

## Solution 1

I enabled Burp to intercept responses, and with the interception on, on hte /lucky.php page, I press `f12` to access developer tools, and inside that I see the button:

```html
<button class='btn block-cube block-cube-hover' id='submit' type='submit' formmethod='post' name='getflag' value='true' disabled>
```

Here I change the disabled to `enabled` and the button is clickable now.

I click the button, and I intercept the request. which now I send to the repeater.

I try to send it a couple of times, but no result, so I send the request to the Intruder.

There I select the `Sniper Attack` and as put the positions on the `getflag=true` in the body, and I add a list of 30 `true` in the payload configuration and start the attack.

One of the responses was longer and the result is `HTB{d154bl3d_bu770n5_w0n7_570p_m3}`.

## Challenge 2

The /admin.php page uses a cookie that has been encoded multiple times. Try to decode the cookie until you get a value with 31-characters. Submit the value as the answer.

## Solution 2

I visit the `/admin.php` two times as for the cookie to be present on the request.

I see the cookie which is `4d325268597a6b7a596a686a5a4449314d4746684f474d7859544d325a6d5a6d597a63355954453359513d3d`.

From the looks of it, it seems to be hex-encoded. In Burp Decoder I choose to `ANSII hex` decode it, and I got:

`M2RhYzkzYjhjZDI1MGFhOGMxYTM2ZmZmYzc5YTE3YQ==` which now looks base64.

I base64-decode it and I got `3dac93b8cd250aa8c1a36fffc79a17a` which is the actual flag.

## Challenge 3

Once you decode the cookie, you will notice that it is only 31 characters long, which appears to be an md5 hash missing its last character. So, try to fuzz the last character of the decoded md5 cookie with all alpha-numeric characters, while encoding each request with the encoding methods you identified above. (You may use the "alphanum-case.txt" wordlist from Seclist for the payload)

## Solution 3

I send the request to the Burp Intruder, and completely remove the cookie value.

I use the `Sniper Attack` and copy the list from the `alphanum-case.txt` wordlist and put that on the payload configuration, which is of the type `list`.

In the `Payload Processing` I add the following:
- Decoded 31-character md5 hash as a suffix
- Encode it in base64
- Encode it as ASCII hex

It's important to keep this order, and I started the attack.

In the response there were some different lengths, and some were much shorter `1285` and in there, the response showed the flag which is `HTB{burp_1n7rud3r_n1nj4!}`.

## Challenge 4

You are using the 'auxiliary/scanner/http/coldfusion_locale_traversal' tool within Metasploit, but it is not working properly for you. You decide to capture the request sent by Metasploit so you can manually verify it and repeat it. Once you capture the request, what is the 'XXXXX' directory being called in '/XXXXX/administrator/..'?

## Solution 4

I start the Metasploit `msfconsole` and use the shown tool.

I set the RHOSTS and RPORT to the server ip address and port for the lab.

Then I set the proxy in order to send the requests to burp:

`set PROXIES http:127.0.0.1:8080` and start the exploit.

On Burp I saw a request which is `GET /CFIDE/administrator/index.cfm HTTP/1.1` so `CFIDE` is the flag.