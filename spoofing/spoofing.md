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
2. Briefly explain different ways in which vulnerability can be exploited.
3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

1. The cookie has `httpOnly: false` so the session cookie is accessible with JavaScript so an attacker can steal this cookie from a malicious site or script. There is also no CSRF protection as the server blindly trusts POST requests from any site which may include potential malicious sites. Also, cookies can also be sent cross-site with forged requests.
2. A user can visit a malicious HTML file such as ` mal-steal-cookie.html` and the attacker can steal the sessionID and the attacker can use this stolen session to impersonate the user. A user can also log in  as an admin and host `mal-csrf.html`. This auto-submits a form so that the browser automatically sends the user's cookie along with the request when it is clicked and since insecure.ts doesn't verify the request's origin, this operation works.
3. In secure.ts, the first problem is solved because of `httpOnly: true` so the cookie can't be accessed with JavaScript. `sameSite: true` forces cookies to only be sent with same-site requests so it blocks CSRF since it prevents cross-site requests from sending the session cookie. 