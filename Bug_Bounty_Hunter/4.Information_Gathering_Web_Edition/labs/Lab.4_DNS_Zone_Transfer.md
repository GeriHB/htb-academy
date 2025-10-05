## Challenge 1

After performing a zone transfer for the domain inlanefreight.htb on the target system, how many DNS records are retrieved from the target system's name server? Provide your answer as an integer, e.g, 123. 

## Solution 1

First modify the `/etc/hosts` file by adding the target ip address to inlanefreight.htb.

Then use the `dig` tool to get the records by:

```sh
dig axfr @10.129.76.214 inlanefreight.htb
```

And this command returns a number of results, which in total is 22 records as shown `;; XFR size: 22 records (messages 1, bytes 594)`.

## Challenge 2

Within the zone record transferred above, find the ip address for ftp.admin.inlanefreight.htb. Respond only with the IP address, eg 127.0.0.1 

## Solution 2

From the results on the first challenge we have the result for this one, which is `ftp.admin.inlanefreight.htb. 604800 IN  A       10.10.34.2`.

## Challenge 3

Within the same zone record, identify the largest IP address allocated within the 10.10.200 IP range. Respond with the full IP address, eg 10.10.200.1 

## Solution 3

The answer to this challenge is also on the results provided from the first challenge, which is `10.10.200.14`.
