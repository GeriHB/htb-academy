## Challenge

After spidering inlanefreight.com, identify the location where future reports will be stored. Respond with the full domain, e.g., files.inlanefreight.com. 

## Solution

I executed the `ReconSpider` tool to crawl the site, by executing this command:

```sh
python3 ReconSpider.py http://inlanefreight.com
```

Among the results provided by the `ReconSpider` tools, in the comments is also the answer to the question in this lab:

```sh
"<!-- TO-DO: change the location of future reports to inlanefreight-comp133.s3.amazonaws.htb -->",
```
