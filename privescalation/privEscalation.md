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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

1. Some potential vulnerabilities include not having authentication and allowing use-controlled role updates. For example, the user doesn't know who is making the request so anyone can claim to be any user by passing in any userId in the request body. Also, the authorization checks if the target user is an admin but not the user making the request so an attacker can simply just pass in the userId of an admin. There are also no restrictions on who can change roles. 
2. A user can simply open the form and submit a POST request to update the role such as updating to an admin role and since there's no real authentication, the server trusts the userId and updates the role. 
3. secure.ts tracks the logged in user using sessions. This allows request to depend on the actual identity and not on the userId is submitted. This allows for session based authentication but secure.ts also has role based authentication. Only the user who is logged in and is an admin can update roles so now, roles can't impersonate admins anyore. Each user is validated separately so that the system only allows changes to valid users. 