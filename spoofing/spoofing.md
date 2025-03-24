# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
 - The secret used to hash the session is hard coded, and therefore anyone with access to this code (this code can be open source) can unhash the session id with the secret
 - The cookie is stored in local storage of the browser, and is configured as  `httpOnly: false`, which means the cookie can be retrived programmatically, that is, by Javascript file in the client side. And if the client has some malware installed in their browser, then their cookie can be accessed by that malware via `document.cookie`
 - The cookie is configured as `sameSite: false`, and therefore it is subject to cross-site request forgery as files from other sites or domains can access the cookie and session id as well

2. Briefly explain different ways in which vulnerability can be exploited.
 - Session hijacking: the attacker can attrack the user to install some malware in their browser (by clicking into a fishing website for example), and that code runs on the same domain and steals session cookie.
  - Cross-site Request Forgery: the attacker can use malware from other sites or domains that can access the cookie and session id, and tricks a user into performing an unwanted action on the website they are already authenticated to, without any user interaction or consent 

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
 - The secret used to hash the session is not hard-coded anymore, but is taken from the command line or configuration file instead, so that the secret is kept confidential.
 - The cookie is configured as `httpOnly: true` and `sameSite: true`, so that it prevents JavaScript codes from accessing cookies, and the corresponding cookie on the client side will only be sent to the server if the client is on the same domain as the server so that cross-site request forgery can be prevented