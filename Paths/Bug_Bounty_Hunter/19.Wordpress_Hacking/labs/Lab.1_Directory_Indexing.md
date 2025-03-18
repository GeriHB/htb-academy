## Challenge 1

Keep in mind the key WordPress directories discussed in the WordPress Structure section. Manually enumerate the target for any directories whose contents can be listed. Browse these directories and locate a flag with the file name flag.txt and submit its contents as the answer.

## Solution 1

The `/wp-content/uploads` is accessible and lists the files and directories.

It has:
- 2020
- 2025
- photo-gallery

All of them are empty, so nothing in there.

Let's enumerate the `/wp-content/` directory, which by itself is not accessible.

```sh
curl -s -X GET http://94.237.59.30:58652 | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/css/bwg-fonts/fonts.css?ver=0.0.1
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/css/sumoselect.min.css?ver=3.0.3
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/css/jquery.mCustomScrollbar.min.css?ver=1.5.34
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/css/styles.min.css?ver=1.5.34
http://94.237.59.30:58652/wp-content/plugins/wp-google-places-review-slider/public/css/wprev-public_combine.css?ver=6.1
http://94.237.59.30:58652/wp-content/plugins/mail-masta/lib/subscriber.js?ver=5.1.6
http://94.237.59.30:58652/wp-content/plugins/mail-masta/lib/jquery.validationEngine-en.js?ver=5.1.6
http://94.237.59.30:58652/wp-content/plugins/mail-masta/lib/jquery.validationEngine.js?ver=5.1.6
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/js/jquery.sumoselect.min.js?ver=3.0.3
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/js/jquery.mobile.min.js?ver=1.3.2
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/js/jquery.mCustomScrollbar.concat.min.js?ver=1.5.34
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/js/jquery.fullscreen-0.4.1.min.js?ver=0.4.1
http://94.237.59.30:58652/wp-content/plugins/photo-gallery/js/scripts.min.js?ver=1.5.34
http://94.237.59.30:58652/wp-content/plugins/wp-google-places-review-slider/public/js/wprev-public-com-min.js?ver=6.1
http://94.237.59.30:58652/wp-content/plugins/mail-masta/lib/css/mm_frontend.css?ver=5.1.6
```

Here we see plugins that are installed, and let's try to access them.

The `flag.txt` was found on the `/wp-content/plugins/mail-masta/inc/` and the content is:

`HTB{3num3r4t10n_15_k3y}`.