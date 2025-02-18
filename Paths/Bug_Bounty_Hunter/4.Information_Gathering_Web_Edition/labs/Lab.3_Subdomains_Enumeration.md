## Challenge

Using the known subdomains for inlanefreight.com (www, ns1, ns2, ns3, blog, support, customer), find any missing subdomains by brute-forcing possible domain names. Provide your answer with the complete subdomain, e.g., www.inlanefreight.com. 

## Solution

I executed the following command to enumerate subdomains using `dnsenum`:

```sh
dnsenum --enum inlanefreight.com -r /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r
```

And among the results are:

```sh
www.inlanefreight.com.                   300      IN    A        134.209.24.248
ns1.inlanefreight.com.                   289      IN    A        178.128.39.165
ns2.inlanefreight.com.                   289      IN    A        206.189.119.186
blog.inlanefreight.com.                  300      IN    A        134.209.24.248
ns3.inlanefreight.com.                   300      IN    A        134.209.24.248
support.inlanefreight.com.               300      IN    A        134.209.24.248
my.inlanefreight.com.                    300      IN    A        134.209.24.248
customer.inlanefreight.com.              300      IN    A        134.209.24.248
```

So the result is `my.inlanefreight.com`.

