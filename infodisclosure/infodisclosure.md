# Information Disclosure

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
The server assumes the user input would be a string, and directly use the input in the database query. But the user input can be some database query or a malicious object instead of a string that will result in the leak of sensitive information like user login data.

2. Briefly explain how a malicious attacker can exploit them.
The attacker can inject some database query like `username[$ne]=` as the input to be executed when the server receives and deals with the request, in order to retrieve sensitive data from the database.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
- It checks if the type of `username` is `string`. This check ensures only string inputs are processed. It blocks malicious query object injection like `username[$ne]=`, which could trigger NoSQL injection and lead to unauthorized data exposure.
- It removes non-alphanumeric characters in the user input so that the username input is sanitized to prevent NoSQL injection.
- It uses a try-catch block to handle error when requesting the user info, this prevents the server from being unavailable, and returning a generic error message avoids accidental leakage of stack traces or database internals