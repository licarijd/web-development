Sessions and JWT 


JWT 

- JSON Web Tokens

- allows us to have protected and unprotected routes 

- users authenticate, receive a token from the server, and use this token to access 
protected pages (eg. an "account" page)


Traditional Cookie Based Authentication 

1) Browser sends a POST/authenticate to a server, with a username and password 

2) Server responds with 

HTTP 200 OK 

Set-Cookie: session = ...

3) Browser sends a GET request for a protected page, eg. /api/account, with 

Cookie session = ... 

4) The Server finds and deserializes the session, and sends a 

HTTP 200 OK { name: "Foo" }


Modern Token-Based Authentication

1) Browser sends a POST/authenticate to a server, with a username and password 

2) Server responds with 

HTTP 200 OK 

{ token: "...JWT..." }

3) Browser sends a GET request for a protected page, eg. /api/account, with 

Authorization: Bearer...JWT...

4) The Server validates the token (the server doesn't have to store the token anywhere,
it simply runs a verify() method on the token; it's stateless in theory), and sends a 

HTTP 200 OK { name: "Foo" }


Advantages of Token-Based Authentication

- it's stateless; the backend doesn't need to store the token in a database, since the 
token is self-contained, including data required to check it's validity 

** that being said, if we do decide to store tokens in a database, like Redis for example,
we could have full control to see what tokens are available, what tokens we've given out,
and can even revoke these tokens at any time. In this case, we'd only generate a tiken when 
a user signs in, and we'd store it in a database.

** the above setup gives us a lot of power and maneuverability when a user signs out; when a user
signs out, we can delete the token from the database.

** But, saving JWTs in a database isn't necessary; in fact, one of the benefits of JWTs is 
that they can be verified through the built in cryptography alone. They don't need to be stored,
which is why they are called "stateless". The question is, do we want to revoke access tokens on 
demand? Read more: https://stackoverflow.com/questions/42763146/does-it-make-sense-to-store-jwt-in-a-database 

- the server's work is simplified 

- can be any type of data, as opposed to just meaningless strings like cookies 
(The piece of data that you hash in your token can be something either a user ID or username
or a much more complex object. In either case, it should be an identifier for a specific user.
Read more: https://www.digitalocean.com/community/tutorials/nodejs-jwt-expressjs)

- a token based approach makes it easier to work with multiple APIs, while cookies are better 
suited for traditional 1 server to client systems (very important in distributed systems)

- the same JWT token can be used accross multiple APIs 

- both browsers and native platforms like iOS and Android can be served, as opposed to 
cookies which don't work well with mobile platforms 


Cons 

- Tokens are larger than cookies (cookies are just strings)

- it can be dangerous if we store important user information in the token, in case it's 
stolen and decoded (although there are security measures which can be taken)


** Example of using JWTs to create an Express authentication system: https://www.digitalocean.com/community/tutorials/nodejs-jwt-expressjs
The example also goes over client-side handling of JWTs, using HttpOnly cookies (which are the
most popular manner for storing auth tokens). It's important to store JWTs client side since they need 
to be sent in the authorization header when we make a reqeust to get authorized data.

** The above system also describes how to create middleware for Express endpoints to authenticate
tokens, which is used to protect GET requests (eg. only serve data to requests with a valid 
JWT). In the Express project, it is useful to create a "middlewares" forlder for Express middleware.
We verify JWTs by checking the Authorization HTTP header. And, we can use Express' "next" parameter 
to keep going down the chain after our middleware function runs (we just call next() in the middleware
function).

More resources on JWTs:

- https://blog.usejournal.com/sessionless-authentication-withe-jwts-with-node-express-passport-js-69b059e4b22c
- https://stackoverflow.com/questions/43452896/authentication-jwt-usage-vs-session
- https://security.stackexchange.com/questions/108662/why-is-bearer-required-before-the-token-in-authorization-header-in-a-http-re


Where to Store JWTs 

The 2 most common approaches for storing JWTs are:

1) Web storage, aka localstorage (persistent even after closing pages) or sessionstorage (data in sessionStorage is cleared
when the page session ends)

2) Cookies with a HttpOnly flag

Web Storage (localStorage/sessionStorage) is accessible through JavaScript on the same domain. This means that any JavaScript
running on your site will have access to web storage, and because of this can be vulnerable to cross-site scripting (XSS)
attacks. XSS, in a nutshell, is a type of vulnerability where an attacker can inject JavaScript that will run on your page.
Basic XSS attacks attempt to inject JavaScript through form inputs, where the attacker puts

<script>alert('You are Hacked');</script>

into a form to see if it is run by the browser and can be viewed by other users. As a storage mechanism, Web Storage does not
enforce any secure standards during transfer. Whoever reads Web Storage and uses it must do their due diligence to
ensure they always send the JWT over HTTPS and never HTTP.

So, web storage isn't great security-wise for storing JWTs!

Cookies are susceptible to Cross Site Request Forgery attacks; malicious sites can hijack your session by getting you to click
on an external link. When you click the link, they steal your session cookie (https://auth0.com/blog/cross-site-request-forgery-csrf/).

Leveraging your web app framework’s CSRF protection makes cookies rock solid for storing a JWT. CSRF can also be partially
prevented by checking the HTTP Referer and Origin header from your API. CSRF attacks will have Referer and Origin headers
that are unrelated to your application.

So TLDR, HttpOnly cookies are best for storing JWTs. More info: https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage


Access Tokens vs Refresh Tokens

So far, we've mainly discussed Access Tokens.

Access Tokens

Access tokens carry the necessary information to access a resource directly. In other words, when a client passes an access
token to a server managing a resource, that server can use the information contained in the token to decide whether the
client is authorized or not. Access tokens usually have an expiration date and are short-lived.

Refresh Tokens 

Refresh tokens carry the information necessary to get a new access token. In other words, whenever an access token is required
to access a specific resource, a client may use a refresh token to get a new access token issued by the authentication server.
Common use cases include getting new access tokens after old ones have expired, or getting access to a new resource for the
first time. Refresh tokens can also expire but are rather long-lived. Refresh tokens are usually subject to strict storage
requirements to ensure they are not leaked. They can also be blacklisted by the authorization server.


Whether the data in tokens are visible or not is usually defined by the implementation. Common implementations allow for
direct authorization checks against an access token. That is, when an access token is passed to a server managing a resource,
the server can read the information contained in the token and decide itself whether the user is authorized or not
(no checks against an authorization server are needed). This is one of the reasons tokens must be signed (using JWS,
for instance). On the other hand, refresh tokens usually require a check against the authorization server. This split
way of handling authorization checks allows for three things:

1) Improved access patterns against the authorization server (lower load, faster checks)
2) Shorter windows of access for leaked access tokens (these expire quickly, reducing the chance of a leaked token allowing access to a protected resource)
3) Sliding-sessions (see below)

So, there are some benefits to using refresh tokens and an authorizatoin server! Long lived refresh tokens are used to generate 
new access tokens whenever they expire.


Sliding-sessions

Sliding-sessions are sessions that expire after a period of inactivity. As you can imagine, this is easily implemented using
access tokens and refresh tokens. When a user performs an action, a new access token is issued. If the user uses an expired
access token, the session is considered inactive and a new access token is required. Whether this token can be obtained with
a refresh token or a new authentication round is required is defined by the requirements of the development team.

Security considerations

Refresh tokens are long-lived. This means when a client gets a refresh token from a server, this token must be stored securely
to keep it from being used by potential attackers. If a refresh token is leaked, it may be used to obtain new access tokens
(and access protected resources) until it is either blacklisted or it expires (which may take a long time). Refresh tokens
must be issued to a single authenticated client to prevent use of leaked tokens by other parties. Access tokens must be kept
secret, but as you may imagine, security considerations are less strict due to their shorter life.

Read more: https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/