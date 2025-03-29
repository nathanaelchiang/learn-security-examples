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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

1. We can see in insecure.ts that the code uses req.query.id from the user id as the id in the MongoDB without validation or satization. It is vulnterable to NoSQL injection.
2. This can lead to a DoS attack because malicious queries can overload the database which will prevent access. The attacker may pass crafted queries such as query that tells MongoDB to return any element where id is not empty. Since id is unique and not empty, this query can end up scanning the entire dataset which can overload the server and cause a denial of service.
3. In secure.ts, we add a rate limit of 5 seconds so that if a user with a certain IP address makes a request within 5 seconds of their previous request, that request is blocked and responds with "Too Many Requests". The try/catch block also catches runtime errors such as bad input or queries to prevent the server from crashing.