# Application Security

## Owasp
* the Open Web Application Security Project is a non-profit
* it is tech-agnostic
* the OWASP Top 10 (Security Risks) is used as a reference for developing and choosing apps

## Further Reading
* [.NET Security Cheat Sheet](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/DotNet_Security_Cheat_Sheet.md)

## Risk Assessment
* risk assessment has several components:
	- overview of the risk
	- understanding the risk
	- common defenses
	- the risk in the wild (real world examples)

## General Concepts
* authorization vs authentication
* you will rarely have to worry about generating cookies, identity etc because you should not be writing your own authentication code. way smarter people will do that -- you just have to keep that information safe and handle it correctly
* string interpolation (expansion of string literal containing placeholders) may create security issues via SQLI, XSS, script injection, etc
	- user input must be properly filtered/escaped
* types of attacks:
	- **DDoS**: Distributed Denial of Service (DDoS)
		* an attempt to make an online service unavailable by overwhelming it with traffic from multiple sources
* transport vulnerability: hypothetically you can send whatever sensitive info you want as long as the transfer method is secure
* **whitelisting** is preferred over **blacklisting** -- if you try to pick things you *shouldn't* do, you'll miss something


# [Owasp's Top Ten](https://owasp.org/www-project-top-ten/)
* or "ten-ish" -- it changes every year
* current state of affairs:
	- injection and broken authentication incidents have dropped
	- most others are on the rise. Broken access control is now #1, cryptographic failures (previously sensitive data exposure) #2
	- some new attacks include insecure design, software and data integrity failures, and server-side request forgery (SSRF)

## Injection
* untrusted data is sent to an interpreter as a part of a command or query in order to trick the interpreter into executing unintended commands
* most commonly we're talking about SQL injection, but there is also LDAP injection
* the stats:
	- attack vector: it can be fairly easy
	- prevalence: average
	- detectability: average
	- impact: severe
* **SQLI**: SQL injection
	- user input is added directly to a SQL query without being sanitized
		```javascript
			passwd = "password’ OR 1=1"
			sql = “SELECT id FROM users WHERE username=’” + uname + “’ AND password=’” + passwd + “’”
			database.execute(sql) // attack results in authentication bypass

			// consider this example: here is a trusted site, with a trusted resource (Widget)
			http://www.mysite.com/Widget?Id=1
			// this is roughly equivalent to:
			SELECT * FROM Widget WHERE ID = 1
			// However that last '1' is untrusted data -- something that can be manipulated
			// for example, here the attacker can get all records (not just where "Id = 1")
			http://www.mysite.com/Widget?Id=1OR1=1
			// This is the basic principle behind attacks to steal CC info and personal information
		```
* using SQLI, an attacker can:
	- bypass authentication
	- access data within the database
	- corrupt or delete the database
	- use a SQLi as the initial vector in an attack of an internal server behind a firewall
	- get the webpage to **exfiltrate data** from the database (get sensitive information back)
* common defenses against SQLi attacks:
	- Whitelist:
		* what input do we trust?
		* does it adhere to expected patterns?
	- use strongly-typed, **parameterized** SQL queries and sprocs
		* separate the query from the input data
		* type cast each parameter
	- or use an ORM -- these are still susceptible to attacks via ad hoc queries
	- fine tune DB permissions
		* segment sql accounts for admin and public so a query for information does not have the permissions to delete anything
		* apply the "principle of least privilege"
	- don't disable [asp.net request validation](https://docs.microsoft.com/en-us/previous-versions/aspnet/hh882339(v=vs.110)) unless you specifically need dangerous input and then ensure that input remains untrusted throughout the stack


## Broken Access Control

## Broken Authentication And Session Management (Identification and Auth Failure)
* in this attack, an attacker can log in and impersonate the victim
* the stats:
	- attack vector: average
	- prevalence: widespread
	- detectability: average
	- impact: severe
* how does this work?
	- when a user authenticates, they send information to the website
	- the attacker can intercept this information and use it to impersonate the user
* types of attacks:
	- **auth cookie theft**:
		* an auth cookie is a small piece of information that a site gives a user when they first log in
		* every time the user comes back to the website to make additional requests, they send the cookie with them
		* if an attacker can intercept the cookie they can use it to impersonate the user
		* they can do this through
			- exploiting XSS risks
			- by "sniffing" it over an insecure connection (`http` rather than `https`)
			- by physically taking it from the user's machine
	- **session id theft**:
		* this type of attack does not require a cookie
		* sometimes the session is represented by a URL string which can simply be copy/pasted
		* the session can sometimes be retrieved from the log
		* the session could be sent out via an insecure email
			- the user themselves might send out the URL to share it not realizing their session is encoded in the URL
	- **account management**:
		* weaknesses in the account manager itself
		* an attacker might **brute-force** the login
			- the attacker tries different passwords over and over and over again
			- the acct management system is not secure enough to say "you're blocked for x time"
			- this is nefarious since most systems allow/use email as the username
		* exploitation of password reset (access to personal information that allows the attacker to reset the password)
		* weak credentials (allowing a 4-number pin as a password)
* common defenses against broken authentication
	- protect cookies
		* ASP.NET: cookies should only use the `HttpOnly` flag which makes it immune to XSS exploitation
		* make sure cookies are flagged as `Secure` -- it cannot be sent over an insecure (`http`) request
	- decrease window of risk
		* re-challenge the user on key actions (changing the password? enter the old one first)
		* do not issue default credentials
	- session management
		* expire sessions quickly (balance with UX)
		* generate new random session id for each session
		* keep session ids out of urls
	- hardening of account management
		* allow and encourage strong passwords
		* two-factor auth
		* implement login rate limiting and lockouts to protect against brute force logins
		* keep login errors vague - do not give attackers more info


## XML External Entities (XXE)
* attackers upload XML containing hostile content to vulnerable XML processors
	- Older XML processors allow specification of an external entity (a URI) that is dereferenced and evaluated during XML processing
	- This can be used to extract data, execute a remote request from the server, execute internal system scan or execute DoS attack
* prevention:
	- use less complex data formats like JSON when possible
	- upgrade XML processors
	- don't use SOAP
	- verify that XML/XSL file upload functionality validates incoming XML
	- disable XML external entity processing in all XML parsers
	- use an XSD to prevent external references from being processed by your application

## XSS: Cross Site Scripting
* cross site scripting, injecting client-side script into a web-page
* the stats:
	- attack vector: average
	- prevalence: widespread
	- detectability: easy
	- impact: moderate
* how does it work?
* what happens?
	- can result in session stealing, account takeovers, MFA bypass or replacement of DOM nodes
	- XSS can also attack the user’s browser with malicious software downloads, key logging, etc
* DOM XSS
	- injection: say your site has something like this -- an attacker could inject malicious script
		```js
			$('#progress-banner').append('searching for <strong>' + searchTerm + '</strong>');
		```
	- this is pretty bad cos this is now "native" local js that is not subject to any of the limitations of cross site scripts and can do whatever
	- we can protect our code by only permitting html text to be updated:
		```js
			$('#progress-banner').append('searching for <strong/>').find("strong").text(query);
		```
* reflected XSS
	- an attacker gives a user a URL with an XSS payload
		* a payload might be included in a URL query string
		* the url can be distributed via social media or posting it publicly (it can be obscured with a bit.ly etc
	- the user clicks through the link to the website using the URL with the XSS payload
		* the XSS is then "reflected" back to the user
		* the XSS can access the data in the user's cookies (auth cookies, say) and exfiltrate that data back to the attacker
	- the attacker can then hijack the session
* XSS injection (stored/persistent XSS)
	- if an attacker can inject XSS into the site's database, the XSS payload will persist, resulting in the same effect as above
* example:
	```javascript
		// this looks like a search for the term "Lager"
		http://www.mysite.com/Search?q=Lager
		// then you might see something like this; the 'Search' page is reflecting that query back to the browser:
		You searched for <strong>Lager</strong>
		// in the above, the domain and the Search resource, and we trust part of the output,
		//  but the search string could be modified to include something like
		<script>alert(document.cookies)</script>
	```
* common defenses:
	- validate input against a white list (white list preferred to black list)
		* should even validate input that is not stored including HTML output that includes unvalidated/unescaped user input
	- always encode output
		* never reflect untrusted data
			- escape things like the `<` symbol with `&lt;` so that script tag cannot be injected
			- it will be un-escaped when it is displayed in the browser
		* this includes data from your own DB -- assume there could be malicious data in your DB
	- encode for context
		* encode differently for HTML/JS/attributes/CSS
		* the wrong encoding in the wrong context is useless
	- request validation in .NET protects us against most XSS attacks

## Insecure Direct Object References
* an attacker changes the data that's sent to a site (such as an account number that maps directly to data in the db), resulting in them gaining access to data they're not supposed to have access to
	```javascript
	// Hitting the Balance resource and sending the AccountId as the query
		http://www.mybank.com/Balance?AccountId=2813775
	```
* the stats:
	- attack vector: easy exploitability
	- prevalence: common
	- detectability: easy
	- impact: moderate
* how does it work?
	- the attacker changes something (like a URL value) which results in access to something they're not supposed to have access to
	- for example, the attacker changes a digit in the URL string
	- this results in a change in the data the website requests from the server
	- the db very accommodatingly sends the record back to the website
	- the attacker is able to exfiltrate that data
* common defenses:
	- implement access controls: _who_ can access to _what_ no matter what the URL is -- **this is most important**
		* be explicit about who can access resources
		* expect the rules to be tested
	- use **indirect reference maps** (like GUIDs)
		* don't expose keys externally (like an account id)
		* map the key to temporary ones -- the temporary key is only used once
		* this is really only for sensitive data, not things like pulling product info from a database
	- avoid predictable keys
		* using ints as keys is dangerous -- an attacker can simply increment, try it, increment, try it etc.
		* natural keys (like username) are discoverable -- use cryptographic strings or global unique identifiers

## Security Misconfiguration
* fairly broad topic, but essentially an attacker accesses an insecure resource
	- like an admin page
	- the website responds with a **gateway risk** -- information that an attacker can use to exploit other risks
	- another example: with a google search for elmah logs, you can gain db passwords, api keys and auth tokens
* the stats:
	- attack vector: easy exploitability
	- prevalence: common
	- detectability: easy
	- impact: moderate (though it's really dependent on the type)
* understanding security misconfiguration
	- out of date software is more vulnerable, including OS, web/app server, dbms, apps and libraries
	- are unnecessary features installed or enabled?
	- default accounts and passwords should be disabled or changed
	- does your error handling reveal stack traces or other sensitive information?
	- are the security settings in your production environment set to secure values? (not still lax like they might be in dev)
* common defenses
	- always harden the install
		* turn off unnecessary features
		* "Principle of least privilege"
	- tune the app security config
		* make sure your prod app is set up correctly compared to dev
		* have distinct credentials for each env
		* defaults for config are often not right
		* use custom logging error messages so your errors cannot be searched
	- ensure packages are up to date
		* be conscious of 3rd party tools
		* have a strategy to monitor and update 3rd party packages
	- institute a repeatable hardening process that can easily be applied to new apps

## Cryptographic Failures (formerly Sensitive Data Exposure)
* previously known as Sensitive Data Exposure, which was a broad symptom rather than a root cause. The renewed focus here is on failures related to cryptography which often leads to sensitive data exposure or system compromise
* when data is not properly protected in transit or at rest
	- sensitive data should not be transmitted or stored in plain text
	- this includes db backups
* the stats:
	- attack vector: difficult exploitability
	- prevalence: uncommon
	- detectability: easy-average
	- impact: severe
* how does it work?
	- **MITM**: Man in the Middle:
		* an attack where the attacker secretly relays and possibly alters the communication between two parties
		* they believe they are directly communicating with each other
		* example: "eavesdropping", in which the attacker makes independent connections with the victims and relays messages between them to make them believe they are talking directly to each other over a private connection, when in fact the entire conversation is controlled by the attacker
* understanding:
	- insufficient use of SSL
		* login not _loaded_ over https
		* cookies not sent correctly -- cookie hijacked
		* "mixed mode" -- the page is loaded over `https` but the javascript is loaded over `http` giving the attacker an opportunity to alter it
	- bad crypto/no crypto (transmitting sensitive info in plain text)
		* incorrect password storage (using symmetric or no encryption)
		* weak or non-existent algorithms
		* poor protection of encryption keys -- crypto is useless if the keys that unlock them are not protected
	- other exposure risks
		* browser auto-complete stores data (like cc numbers which a subsequent user can see)
		* disclosed via URL (accidental sharing of URL by user over social media)
		* leaked via logs
* common defenses
	- minimize sensitive data collection
		* "if you don't have it, you can't lose it"
		* reduce the window of storage -- get rid of it when you no longer need it
	- apply `https` everywhere
		* it's too easy to insufficiently implement
		* start with it everywhere -- it's not that hard!
	- use strong crypto
		* salted hashing - SHA-256
		* encryption for cardholder data: AES-256 (CDS at rest), SQL Server EKM + Townsend Alliance Key Manager
		* use hashing algorithms designed for passwords
		* be very careful with key management


## Missing Function Level Access Control (Failure To Restrict Url Access)
* authorized user gains access to a part of the system they should not have access to. users with no auth may also be able to gain access
* the stats:
	- attack vector: easy -- just plug in a URL
	- prevalence: common
	- detectability: average
	- impact: moderate
* how it works:
	- suppose we have a user (an admin) and they make a request to the website, and the website sends them to a secure page as designated by the URL it navigates to -- if they weren't logged in, they wouldn't receive the URL
	- an attacker requests the admin page and gets it
	- the admin resource doesn't have its own access control -- it's just assumed that if you have the URL, you are supposed to access that content
* understanding missing function level access control
	- does the UI show navigation to unauthorized functions?
	- are server-side authentication or authorization checks missing? -- you can't just authorize on the client -- it's too easily circumvented. authorization must occur on the server
	- are server-side checks done that solely rely on info that could be provided by an attacker? ("untrusted data")
	- are system or diagnostic resources accessible without proper authorization?
	- will "forced browsing" (an attacker enumerating through resources that are commonly available but not visible, like `admin.php`) disclose unsecure resources?
* common defenses
	- define a clear authorization model (RBAC: Role-based access control)
		* define centrally and consistently
		* use roles and apply membership
		* function-level access checks
	- check for forced browsing
		* check for default framework resources (tracing for debugging, admin, etc)
		* automated scanners are excellent for this
	- always test unprivileged roles
		* capture and replay privileged requests from a user with elevated permissions as a user with lesser permissions
		* include POST requests and async calls
	- ASP.NET membership provider / IIS Integrated Pipeline
	- rate limit API and controller access to minimize harm
	- include funtional access control unit and integration tests
	- log all access control failures. alert admins when repeated failures are detected
	- invalidate auth tokens (like JWTs) on the server after logout
	- Disable web server directory listing and don’t store file metadata and backup files within web roots.
	- Create access controls that enforce record ownership, rather than accepting that the user can create, read, update, or delete any record.

## Cross Site Request Forgery (CSRF)
* [cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
* [dotnet documentation on CSRF](https://learn.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
* the stats:
	- attack vector: average
	- prevalence: common
	- detectability: easy
	- impact: moderate
* two basic types:
	- "do stuff" request forgery
	- "get stuff" request forgery
* how it works:
	- the attacker has a website and sends a user a link via social media, phishing, etc
	- the user visits the malicious website which makes a malicious request to the target website wherein the user is already logged in/has a valid cookie
	- so essentially the objective it to get the user to make a request on the attacker's behalf on a website the user is already logged into and that the attacker can't access otherwise
* understanding CSRF
	- a browser/client has a cookie cached so that when a request is made to a server, the identity of the client requesting the data is known
	- but from the server's perspective, where is the request actually coming from? it might not be coming from the user's browser, but from an attacker's website: a user could be tricked into visiting the malicious site which makes the request to the target server using the user's cookie for that service. to the server, this request is coming from the genuine user (because it is, kind of)
* example
	- suppose a user transfers money online
		```
			HTTP POST https://www.mybank.com/transfer
		```
	- this request has an auth-cookie attached to it (as it is with every request the user makes) as well as a request body containing the request amount, # of the account to transfer to, etc
	- if the attacker is successful in a CSRF attack, they can fabricate the above post including the request body with the amount and the account they want the money to go to -- _all they need is the auth cookie to complete the request_
	- in this scenario, the attacker doesn't need to worry about same-origin policies because this is a "do stuff" attack
* common defenses
	- same origin policy: browser security features to the rescue!
		* if you're making a request to a reddit server, the request must originate from reddit.com
		* a malicious website can get a response from the reddit server, but the browser will see that the response did not originate from the malicious site -- the browser blocks it so the malicious site will never see the response content
		* however because the request to the server is completed (and blocked), this only helps with the **get stuff** variety of CSRF attacks
	- employ anti-forgery tokens (**Cross Site Request Forgery Token**)
		* this protects against **do stuff** requests
		* the whole attack pattern is very predictable -- here's a URL, a cookie and a req body, and you know how to forge a request
		* an anti-forgery token injects randomness that the attacker cannot predict nor fabricate
		* the anti-forgery token is not a part of the user cookie -- where the heck does it come from?
			- **to-do**: the token can't just live in client code -- it would be visible to attackers. how is it stored/retrieved by legitimate clients?
		* it is sent along with the auth token -- these two pieces of data together should be used to secure any endpoint that modifies data or has any side effects
		* the (legitimate) client js code knows to attach the anti-forgery token to these requests -- the malicious site won't know to do that, or it won't know the token (hopefully)
	- validate the referrer
		* the referrer -- the attacker's website -- is embedded in the request header
		* valid that requests don't originate externally
		* however referrer can be spoofed so this is only helpful in combination with other measures
	- other:
		* native browser defenses (CORS)
		* fraud detection patterns (no activity all day and then all of a sudden the user is transferring $1000000?)

## Insecure Deserialization
* this type of attack is difficult to implement, but if successful it can result in data tampering, access control, DoS attack or remote code execution attack
* Applications and APIs may be vulnerable if they deserialize hostile or tampered objects supplied by an attacker
* prevention:
	- do not accept serialized objects from untrusted sources
	- enforce strict type constraints during deserialization
	- isolate and run code in low privilege environments
	- log deserialization failures and exceptions to detect attempts; monitor deserialization and detect if a particular user does it constantly
	- implement integrity checks (like digital signatures)
	- restrict/monitor network connectivity with servers/containers that deserialize


## Using Components With Known Vulnerabilities
* the stats:
	- attack vector: average
	- prevalence: widespread
	- detectability: difficult
	- impact: moderate
* how it works:
	- a vulnerable component is a product or library which is vulnerable for any of the following
		* allows circumvention access control
		* local file inclusion
		* SQL injection, XSS, CSRF
		* vulnerable to brute force login
	- components usually run with the same perms as the application itself so a compromised component can be disasterous
* understanding:
	- an attacker makes a request for a page that uses a potentially vulnerable component
	- the website responds, disclosing the version of the component (html source, comments, implicitly)
	- the attacker researches known vulnerabilities and then attacks (sometimes in a matter of minutes)
* defenses
	- identify components and versions including nested dependencies
		* use official sources over secure links
		* components are often used haphazardly -- don't use more than you need
		* keep track of components and versions -- know what you're using so you can check if you have vulnerabilities
		* only use components that are maintained and patched
	- components should be monitored
		* monitor **CVEs** which post information about vulnerabilities
		* keep abreast of project updates
	- keep components updated
		* use framework's package management to keep packages updated
		* regularly monitor new releases
	- so: know what you have, don't have more than what you need, and make it easy to update what you have

## Insufficient Logging And Monitoring
* high value transactions, logins and login failures should be logged and monitored
* this information should be visible to you but not accessible by attackers
* create a response plan

## Unvalidated Redirects And Forwards
* the stats:
	- attack vector: average
	- prevalence: uncommon
	- detectability: easy
	- impact: moderate
* how it works:
	- an attacker sends a URL with a redirect payload to a user
	- the user thinks it's legit and clicks it
	- the URL redirects the user form the site they think they're going to to the malicious website
* understanding
	- many devs might embed something like this to log traffic or activity on their website
		`/redirect/?url=http://externalwebsite.com`
	- the attacker can manipulate this link to have the redirect point to whatever they want (`http://www.attackersite.com/malware.exe`)
	- what does this accomplish? it allows them to trick users into visiting an _untrusted_ site by hijacking a _trusted_ one
* this is clearly not in the same class as other attacks, but worth mentioning anyway
* defenses
	- use a URL whitelist
		* what are URLS allowed to redirect to
		* abort if the URL is not allowed
	- use indirect references
		* pass an id to redirect, not a verbatim URL
		* resolve the URL from a reference map
	- check the referrer
		* did the redirect originate from the site?
		* may need to whitelist multiple sites
