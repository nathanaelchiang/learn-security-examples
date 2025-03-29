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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

1. insecure.ts is vulnerable to XSS. Cross-Site Scripting takes input from the user from the /register form and it injects it into HTML without validation or sanitization. Since req.session.user is displayed in the response HTML, any JS code submitted as name will be executed in the user's browser.
2. A malicious attacker can input a malicious script that gets stored in the session as the user value which will get rendered on the homepage so the browser ends up executing the malicious script which can cause tampering. 
3. secure.ts sanitizes the user input by using escapeHTML() to help solve the XSS vulnerability as escapeHTML() replaces certain characters so that malicious scripts get rendered as plain text and not executable code. 