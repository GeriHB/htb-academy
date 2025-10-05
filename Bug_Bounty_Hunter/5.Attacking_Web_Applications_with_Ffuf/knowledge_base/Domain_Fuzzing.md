# Domain Fuzzing

## DNS Records

Browsers know only how to go to the IP addresses, and if we provide a URL they will check locally `/etc/hosts/` and the public DNS.

We can add an ip to our local hosts file via:

```sh
sudo sh -c 'echo "IP_Address WebsiteName" >> /etc/hosts'
```

## Sub-domain Fuzzing

Is the same as directory fuzzing, just use a wordlist dedicated to that, and put FUZZ before the actual domain:

```sh
ffuf -w Wordlist:FUZZ -u http://FUZZ.ip_address:PORT
```

## Vhost Fuzzing

`Vhost` is a sub-domain on the same server and has the same IP. So, a single IP address can serve multiple different websites.

A note: By sub-domain fuzzing we can only see the public sub-domains~

But, we can use `VHosts Fuzzing` on an IP that we have, and identify public and non-public sub-domains and VHosts.

To scan for VHosts we should add the header option `-H`:
```sh
ffuf -w WORDLIST:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
```

NOTE: We will always have `200 OK` but we should check the page size.

## Filtering results

We can filter the response size, and for example if the incorrect ones are above 900 we can filter it out with `-fs 900`.

```sh
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900
```
