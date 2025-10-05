## Challenge

 The string found in the attached file has been encoded several times with various encoders. Try to use the decoding tools we discussed to decode it and get the flag.

 ## Solution

 The encoded flag is `VTJ4U1VrNUZjRlZXVkVKTFZrWkdOVk5zVW10aFZYQlZWRmh3UzFaR2NITlRiRkphWld0d1ZWUllaRXRXUm10M1UyeFNUbVZGY0ZWWGJYaExWa1V3ZVZOc1VsZGlWWEJWVjIxNFMxWkZNVFJUYkZKaFlrVndWVmR0YUV0V1JUQjNVMnhTYTJGM1BUMD0=` and I put that on the Burp Decoder, to decode it from `base64`.

 The decoded text which seems to still be a base64 encoded string is:
`VTJ4U1VrNUZjRlZXVkVKTFZrWkdOVk5zVW10aFZYQlZWRmh3UzFaR2NITlRiRkphWld0d1ZWUllaRXRXUm10M1UyeFNUbVZGY0ZWWGJYaExWa1V3ZVZOc1VsZGlWWEJWVjIxNFMxWkZNVFJUYkZKaFlrVndWVmR0YUV0V1JUQjNVMnhTYTJGM1BUMD0=`

Afte decoding it again with base64, I got a `SlRRNEpUVTBKVFF5SlRkaUpUTXpKVFpsSlRZekpUTXdKVFkwSlRNeEpUWmxKVE0ySlRWbUpUWmxKVE14SlRabEpUWmhKVE0wSlRkaw==` and I try again a base64 decoding process, which gives me `JTQ4JTU0JTQyJTdiJTMzJTZlJTYzJTMwJTY0JTMxJTZlJTM2JTVmJTZlJTMxJTZlJTZhJTM0JTdk`

I repeat the process again to base64 decode it and it provides with the `%48%54%42%7b%33%6e%63%30%64%31%6e%36%5f%6e%31%6e%6a%34%7d` url-encoded string.

Now I url-decode it and it produces the flag: `HTB{3nc0d1n6_n1nj4}`

