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

- the server's work is simplified 

- can be any type of data, as opposed to just meaningless strings like cookies 

- a token based approach makes it easier to work with multiple APIs, while cookies are better 
suited for traditional 1 server to client systems

- the same JWT token can be used accross multiple APIs 

- both browsers and native platforms like iOS and Android can be served, as opposed to 
cookies which don't work well with mobile platforms 


Cons 

- Tokens are larger than cookies (cookies are just strings)

- it can be dangerous if we store important user information in the token, in case it's 
stolen and decoded (although there are security measures which can be taken)

