# Server-side Request Forgery

Occurs when a web app fetches additional resources from a remote location based on user-supplied data, such as URL.

The server fetches remote resources based on user input - the attacker coerces the server into making requests to arbitrary URLs supplied by the attacker.

**Identifying SSRF**
Looking at a web app example which has a functionality to schedule appointments.

We see the request on Burp and see that the `POST` request has a body that contains a parameter with a url as a value.

We start a listening server, and put our ip we see the request on our server.

We can **enumerate** the system, by making requests to different ports.

This process can be done with `ffuf`, by creating a list of ports that we want to test and then fuzz all open ports by filtering out responses containing the error message.

```sh
ffuf -w ./ports.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect to"
```

