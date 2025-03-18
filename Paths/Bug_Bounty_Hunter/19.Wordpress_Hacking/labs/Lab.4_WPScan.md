
## Challenge 1

Enumerate the provided WordPress instance for all installed plugins. Perform a scan with WPScan against the target and submit the version of the vulnerable plugin named “photo-gallery”.

## Solution

Run the WPScan to enumerate the plugins.

```sh
wpscan --url http://94.237.59.30:58652 --enumerate ap
```

And it provides with the information about the plugins, such as:

```sh
+] photo-gallery
 | Location: http://94.237.59.30:58652/wp-content/plugins/photo-gallery/
 | Last Updated: 2025-02-26T17:51:00.000Z
 | [!] The version is out of date, the latest version is 1.8.34
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Urls In 404 Page (Passive Detection)
 |
 | Version: 1.5.34 (100% confidence)
```
