# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
On line 58, the server implicitly assumes the user input would be a numeric or a string, and it directly reflects user input (`req.session.user`) into the HTML response without proper sanitization or encoding. If a user inputs a string containing HTML or Javascript, it can be executed in the browser when rendered. This creates a potential XSS (Cross-Site Scripting) vulnerability. 

2. Briefly explain how a malicious attacker can exploit them.
The attacker can inject some HTML or Javscript scripts in the input, for example with some link to a malacious website. When the server renders the homepage, this script will be executed in the victim’s browser. The attacker could redirect users to malicious websites, steal session cookies, or perform actions on behalf of the user — all without their knowledge.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
It uses `escapeHTML()` function to escape input with HTML special characters. This prevents users from injecting malicious scripts that could be executed in the browser. As a result, the input is not executable anymore, and but displayed as plain text instead.