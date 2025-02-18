## Challenge 1

Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "web"? Answer using the full domain, e.g. "x.inlanefreight.htb" 

## Solution 1

I added the target ip to the `/etc/hosts` file and started the enumeration with the following command:

```sh
gobuster vhost -u http://inlanefreight.htb:38211 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 100
```

I made the threads to 100 as it was much faster.

Then I got the results shown below, which have answers for all of the challenges on this section:

```sh
Found: blog.inlanefreight.htb:38211 Status: 200 [Size: 98]
Found: admin.inlanefreight.htb:38211 Status: 200 [Size: 100]
Found: support.inlanefreight.htb:38211 Status: 200 [Size: 104]
Found: forum.inlanefreight.htb:38211 Status: 200 [Size: 100]
Found: vm5.inlanefreight.htb:38211 Status: 200 [Size: 96]
Found: browse.inlanefreight.htb:38211 Status: 200 [Size: 102]
Found: web17611.inlanefreight.htb:38211 Status: 200 [Size: 106]
```

## Challenge 2

Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "vm"? Answer using the full domain, e.g. "x.inlanefreight.htb"

## Challenge 3

Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "br"? Answer using the full domain, e.g. "x.inlanefreight.htb" 

## Challenge 4

Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "a"? Answer using the full domain, e.g. "x.inlanefreight.htb" 

## Challenge 5

Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "su"? Answer using the full domain, e.g. "x.inlanefreight.htb" 