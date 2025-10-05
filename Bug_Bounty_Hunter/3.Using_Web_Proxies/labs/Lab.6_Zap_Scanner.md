## Challenge
Run ZAP Scanner on the target above to identify directories and potential vulnerabilities. Once you find the high-level vulnerability, try to use it to read the flag at '/flag.txt'

## Solution
After doing the ZAP Scanner There was a `Remote OS Command Injection` vulnerability on `94.237.59.180:51690/devtools/ping?ip=127.0.0.1%26cat+%2Fetc%2Fpasswd%26`

Now let's try to read the flag which is `HTB{5c4nn3r5_f1nd_vuln5_w3_m155}`