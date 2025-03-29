# Tampering

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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

1. ```const user = await User.findOne({ username: username as string }).exec();```
This line of code is insecure because it is vulnerable to NosQL inection. It passes the user input from req.query directly into the MongoDB query without validation or sanitization.
2. This can lead to an information disclosure attack because an attack query can inject a query that tells MongoDB to return any user whose username is not equal to an empty string which may disclose the usernames in the database. This causes the server to disclose sensitive user information without proper authentication.
3. The secure.ts does type checking to make sure that username is a string and not a complex object such as a query. We also use input sanitization to remove non-alphanumeric characters to help guard against potential injections. The try-catch block also handles errors gracefully to help avoid leaking internal server info.