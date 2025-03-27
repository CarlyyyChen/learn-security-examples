# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.  
The server does not have repudiation. It does not log requests with user identities. The attacker could repudiate the message and attack the server, then deny having sent it, and the server wouldn't be able to prove otherwise.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.  
It implements a middleware that is applicable to all routes to log the timestamp, HTTP method and endpoint, user data (username) and ip address of the request. As a result, an audit trail with user/IP/timestamp is created.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?  
Chain of responsibility. The chain of responsibility pattern allows the server to pass a request along a chain of handlers until one of them handles it. Each handler decides whether to handle the request, or pass it to the next handler in the chain. Here the server in the secure version implements a middleware to log requests, so that each request will be passed to this handler before passing to the next function. And this handler will log the request and then pass it to the next handler.
