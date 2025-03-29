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
2. Briefly explain why the vulnerability is addressed in __secure.ts__.
3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

1. The vulnerability is that thre isn't logging so it's susceptible to repudiation attacks. In insecure.ts, we see that any user can send or retrieve messages without authentication. There is no logging to track who performed what action so users can send offensive messages or tamper with data and they can deny their actions because there isn't proper logging.
2. secure.ts checks for authentication. The /get-messages route checks that it is authenticated first, and then logs every request using middleware and actions like sending or retrieving messages are also logged with the user and IP address. 
3. This is the middleware pattern as it intercepts HTTP requests before they reach the route handlers. The logging middleware that we use gets the time, method, URL, and IP for every request who allows for consistent logging. 