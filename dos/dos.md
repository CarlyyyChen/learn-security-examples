# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
- It does not use a rate limiter so the attacker can send a lot of requests to overwhelm the server and make it unavailable.
- It does not properly handle errors when requesting for user info. The server assumes that `req.query.id` will always be a valid MongoDB ObjectId string. However, it does not validate this input. If the attacker inputs something like `id[$ne]=`, this will cause a NoSQL injection, and this query will cause the server error, making it crash and down for all other users.

2. Briefly explain how a malicious attacker can exploit them.
An attacker can perform a flood attack by sending thousands of requests per second to `/userinfo`, exhausting server or database capacity. Since there's no rate limiting or throttling, the server keeps processing all requests until it becomes unresponsive.
They can also inject malicious codes like `id[$ne]=` to cause server error, resulting in a denial of service for other users.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
- It implements a middleware to limit the request frequencies, and therefore preventing attackers from flooding the server with high-frequency traffic.
- It surrounds the database query with a try-catch block. It prevents unhandled exceptions from crashing the server when invalid or malformed input is provided.