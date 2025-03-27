# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
 - There is no real authentication — anyone can claim any `userId`. The role check is done based on the input user (`userId`), not the identity of the authenticated user. 
 - The `users` data is stored in memory, anyone with access to it knows which `userId` is admin so that they can perform privilege escalation to pretent to be admin and manipulate user role data. 
 - It does not sanitize or restrict malicious input. It is vulnerable to injection attacks.
 - There is no logging or audit trail. It is vulnerable to repudiation.

2. Briefly explain how a malicious attacker can exploit them.  
The attacker can enter `1` as the userId and update the user role to `user` so that the true admin will lose their administrative rights.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
- The session id is required to authenticate the user before they can update user roles. Only users with a valid session (`req.session.userId`) are allowed to perform sensitive operations. This ensures requests come from an authenticated session — not from forged or arbitrary requests.
- The session cookie is configured as `httpOnly: true` and `sameSite: "strict"` so tha it prevents Javascript access to cookies and CSRF via cross-site requests.
- Even if a user is authenticated, the system checks whether the logged-in user has the `admin` role before allowing them to update another user’s role. This prevents regular users from promoting themselves or others.
- Before changing a role, the server checks if the target userId exists in the system. This prevents attackers from submitting fake IDs or modifying non-existent users.
