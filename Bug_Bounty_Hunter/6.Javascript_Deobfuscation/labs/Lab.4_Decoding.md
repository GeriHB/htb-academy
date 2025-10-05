## Challenge
Using what you learned in this section, determine the type of encoding used in the string you got at previous exercise, and decode it. To get the flag, you can send a 'POST' request to 'serial.php', and set the data as "serial=YOUR_DECODED_OUTPUT". 

## Solution
The string from previous lab was `N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz`

By the looks of it is not `hex` as it has other characters as well, such as `N` for example.

Also, there is a low probability to be `Rot13`, so it should be base 64, let's try to decode it.

```sh
echo N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz | base64 -d
```

And the result is: `7h15_15_a_s3cr37_m3554g3`

Now let's send a `POST` request with this as a value to `serial`:

```sh
curl -s 83.136.249.139:45351/serial.php -X POST -d "serial=7h15_15_a_s3cr37_m3554g3"
```

and the result gives us the flag: `HTB{ju57_4n07h3r_r4nd0m_53r14l}`.

