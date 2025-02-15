## Challenge
 The directory we found above sets the cookie to the md5 hash of the username, as we can see the md5 cookie in the request for the (guest) user. Visit '/skills/' to get a request with a cookie, then try to use ZAP Fuzzer to fuzz the cookie for different md5 hashed usernames to get the flag. Use the "top-usernames-shortlist.txt" wordlist from Seclists.

 ## Solution

 I have used Burp here as well. It is an app I'm used to it, and feel comfortable using it.

Visit the page at `/skills/` two times and you should see a `Welcome Guest`, send that to the Burp Intruder, and there you should have a cookie for the `guest` user.

Add payload positions on it, `Cookie: cookie=084e0343a0486ff05530df6c705c8bb4`, then select the `Sniper Attack` and a `Simple List`, where you should paste the contents of the wordlist mentioned in the challenge.

In the Payload Processing choose the `hash` and choose `md5`, and then start the attack.

It should be finished really fast, and now you see there are all of them `200 OK` but that's not a problem. Check out the length, and it should be some which are more lengthy.

Click on those and one should be `Guest` and the other one is `user` where you have the flag: `HTB{fuzz1n6_my_f1r57_c00k13}`