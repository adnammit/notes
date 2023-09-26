# Auth

## Resources
* [OIDC, OAuth and SAML](https://www.okta.com/identity-101/whats-the-difference-between-oauth-openid-connect-and-saml/)
* [id and access tokens](https://auth0.com/blog/id-token-access-token-what-is-the-difference)
* [scopes](https://auth0.com/docs/get-started/apis/scopes)
* [jwt.io](https://jwt.io/)
* [crash course in OAuth/OIDC from IdentityServer Founder](https://youtu.be/Ps8ep-glDfc)
* **START HERE** https://vimeo.com/113604459 12:42


## Key Concepts
* **authentication**: the process of verifying the identity of a user
* **authorization**: the process of determining whether a user is allowed to perform a given action
* **federated identity**: the ability to share identity information across multiple systems using a single set of credentials
* **public vs confidential clients**: a confidential client is an application which is able to securely store credentials like a client id and client secret. a public client is an application which is not able to securely store credentials, such as apps running in a browser or on mobile
* **service provider/client**: the application that the user is trying to access/the app that initiates the authentication process. maybe the same thing, maybe not
* **OAuth**: an authorization protocol that allows a website or app to access resources hosted by another application on behalf of the user using access tokens
	* OAuth == access tokens: "get the resources"
	* **resource owner**: the user who's protected resources need to be accessed
	* **resource server**: the API or service that protects the resources
	* **authorization server**: the server that authorizes the user/client to access the resource server
	* **access token**: a credential that allows one app to access a user's protected resources in another app. the client passes the access token obtained from the auth server to the resource server for validation and interpretation
	* **scopes** are used to control what the client app can and cannot do on behalf of the user
* **OIDC**: an identity layer on top of OAuth2 that allows a website or app to verify the identity of a user using identity tokens
	* OIDC == identity tokens: "prove the user's identity"
	* **identity provider**: the service that manages the user's identity
	* **id token**: OIDC standard; a signed JWT that contains proof of the user's identity
	* **claims** are key-value pairs that contain information about the user and their authentication operation
* **JWT**: encoded JSON used for multiple authentication and authorization transactions


## Overview
* in ye olden days (and for some applications presently) [**Windows Authentication**](https://docs.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview) (aka NTLM or kerberos) is used
	- the client browser displays a login dialog box to capture username and password
	- the client sends the encoded (note: *not* encrypted) credentials back to the server where IIS directly handles authentication
	- Windows Auth is well-suited to intranet environments:
		* client and server are in the same domain
		* http proxy connections are not required
	- [more info here](https://www.c-sharpcorner.com/UploadFile/84c85b/understanding-windows-authentication-in-detail/)
* SAML is the mother of modern authentication
* these days authentication tends to be handled with some flavor of **OIDC**

## Single Sign-On (SSO), SAML, OAuth and OIDC
* SSO allows users to access many applications using the same identity
* we have an app we need to access, like YouTube -- this is the **service provider**
* and we have a separate service which manages the user identity -- this is the **identity provider** or IDP
* SSO uses the concept of **federated identity** which allows sharing of identity information across trusted but independent systems
* SSO can be implemented using SAML or OAuth (OIDC), or both
	- OAuth is an authorization process
	- SAML and OIDC are both standards for federated authentication
* when talking to an API, we tend to use **OAuth2** authorization
	- OAuth2 is not actually an authentication protocol -- it's just for securely passing access tokens (especially from clients which are inherently untrustworthy)
	- _OAuth2 by itself should only be used to get info from an API, it should never be used to sign into a client application_ -- it is not for authorization
* **TODO this next bullet doesn't make sense**
* distinguishing **identity** and **access**
	- IdentityServer and Azure AD implement the OAuth2 standard -- they are **identity providers** (IDPs)
	- this is as opposed to **authorization servers**
	- more and more, even if you just need an access token, you're probably working with OIDC

### OIDC or SAML SSO?
* many IDPs provide support for both, including Duo, JumpCloud, Okta, MS and onelogin
* boils down to which is easier to integrate with your application
* if you're creating a new web app and are integrating with other OIDC providers, OIDC is probably the way to go

## OAuth
* remember that OAuth is *not* an authentication protocol -- it's just for securely passing access tokens (especially from clients which are inherently untrustworthy)
* OAuth allows an app to access a user's resources on behalf of the user without exposing the user's password -- the third-party app is *authorized* to access the resource

### OAuth Architecture
* OAuth architecture consists of a **resource server** (the API or service that protects the resources) and an **authorization server** (the server that authorizes the user/client to access the resource server). these two servers are always owned by the same entity -- the manager of the resource must manage and protect access to it -- there must be trust
* then there are third party apps who need to access the resource on behalf of the user -- these are the **clients**
	- clients may be confidential (server) or public (mobile/browser)
* typical flow overview:
	1. client makes a call to the auth server to get a token ("authorize" endpoint) using an api key
	2. auth server returns access token with appropriate scopes, expiration, etc
	3. client uses the token to access the resource server

### OAuth Flows
* we use two flows based on two different behaviors: interactive (with a user) or non-interactive (server-to-server)

#### Client Credentials Flow
* machine-to-machine
* more simple: only authenticates the client
* sometimes called "two-legged flow"
* the client must be pre-registered with the auth server
* flow:
	- the client makes a request to the auth server 
		* typically the request specifies the encoded **client id** and **client secret** in the auth header but there are other ways of doing it (certificates, pub/private key pair, etc)
		* request also includes grant type: `grant_type=client_credentials` and requested scopes (?)
	- the auth server validates that the client is allowed to access the api (see pre-registration step)
	- the auth server returns a json response body with the access token (see **Access Tokens** for more detail)
	- the client makes subsequent calls to the resource server with the access token in the auth header (`Authorization: Bearer {token}`)


#### Authorization Code Flow
* interactive applications
* more complex
* suitable for any type of interactive client application
* allows for UI during protocol flow (login, consent)
* dependent on browser
* user-centric access tokens and support for refresh tokens
* requires use of PKCE extension
* consists of two logical steps:
	- **front channel**: what the user sees in the browser: login, consent, etc
		* browser sends a request to the auth server (`/authorize` endpoint) that includes the client id, requested scopes, redirect uri, and `response_type=code`
			- **TODO** is this a request or is it a redirect to the authorize page hosted by the auth server?
		* browser redirects to a page owned by the IDP and the user logs in and consents to the requested scopes (assuming the client is a third-party app)
		* the auth server sends back an **auth code** which means nothing, but the auth server can associate it with the user and the client
	- **back channel**: what happens behind the scenes: token exchange, etc
		* once the auth code is received, the client server makes a request to the auth server (`/token` endpoint) that includes the auth code, client id, client secret, redirect uri, and `grant_type=authorization_code`
		* the auth server receives the request and validates everything in the request against the initial data used to get the auth code in the front channel
		* the auth server then returns an access token and a refresh token
		* the client server stores the data server-side where it is cached and used to access the resource server
* when the access token expires, you don't want to redirect the user to the auth server -- you can use a **refresh token** to get a fresh token in the background
	- the client receives a 401
	- client posts to `/token` endpoint with `grant_type=refresh_token` and the refresh token (and client id and secret)
	- client receives a new access and refresh token pair to cache


## OpenId Connect (OIDC)
* most common in the internet at large. example: using google to sign in to YouTube
* it's an identity layer on top of OAuth2 -- **OpenId extends OAuth2**
* OIDC SSO is similar to SAML SSO but instead of XML we use JSON (JWTs)
* standardizes token type -- makes everything more inter-connectable
	- defines identity tokens (**JSON Web Tokens** or **JWTs**)
	- defines standard cryptography
	- defines token validation procedure
* combines authentication with delegated API access -- since it sits on OAuth2, we have *authentication and access control in a single protocol*, without needing to learn OAuth2 -- just implement OpenID
* also because it is combined with OAuth2, there's only one trip to the server
* not really that simple to implement either

### How OpenId Connect Works
* a client application can request an identity token (alongside an access token if needed)
* the identity token is used to sign in to the client application
* the access token can be used to access an API
* it defines a `UserInfo` endpoint allows a client application to get additional information on the user
* flow of how it works:
	- a client app (**relying party**) needs an id token
	- an authentication request is made to the IDP
		* the user is redirected to the identity provider
		* the user proves who they are (by providing a user/pw combo)
	- the IDP creates an id token and signs it
	- the user is redirected back to the client application
		* the client application receives the identity token and validates it
		* if the validation checks out, the client now has proof of identity
		* if the client is an ASP.NET app, a claims identity is created
			- stored as an authentication ticket (?) in an encrypted cookie
			- the browser then sends that cookie on each request to the client application
* **public** and **confidential** clients
	- confidential clients
		* apps that are live on the server (protected)
		* they are capable of confidentially storing the credentials like `clientid` and `clientsecret`
	- public clients
		* executing in a browser or mobile app
		* e.g. js, angular, mobile apps
		* incapable of maintaining confidentiality

### How Is This Secure?
* all of this transfer of sensitive data is handled over **TLS (SSL)**
* the redirect to the provider should take place in a new window (rather than being embedded in the web app) so the app cannot snoop on the user's credentials
* apps consuming tokens independently validate their authenticity using the issuer's public key

### OIDC Endpoints
* authorization endpoint (IDP level)
	- used by the client app to obtain access/identity tokens via redirection
* redirection endpoint (client level)
	- the client endpoint that the user is redirected to after receiving the code/token(s) from the IDP
* token endpoint (IDP level)
	- clients can programmatically request tokens from the IDP via HTTP POST without redirection

### OIDC Flows
* you can technically use many things in places you shouldn't be using them -- pick the right one!
* **authorization code**
	- gets auth code from the auth endpoint and tokens from the token endpoint
	- suitable for confidential clients
	- long-lived access
	- short-lived, single use credential (this is the same person who started the flow at the level of the web app)
* **implicit flow**
	- returns tokens from auth endpoint -- no use of token endpoint
	- suitable for public clients, though confidential clients may also use it
	- no client auth
	- no long-lived access
* **hybrid flow**
	- gets tokens from auth endpoint and token endpoint
	- suitable for confidential clients
	- long-lived access
	- sometimes allowed for native apps even though they're technically public clients
	- _hybrid is advised where suitable_
		* allows us to get an identity token from the auth endpoint first
		* we can then verify that token before making additional round trips to get an access token
		* the id and access tokens are linked together
* **client credentials flow**
	- used for machine-to-machine communication
	- https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow

## Tokens
* any token should be validated using the issuer's public key before use

### JSON Web Token (JWT)
* OIDC standard
* they are not encrypted, they are just base64 encoded
* many other types of tokens are implemented as JWTs

### Identity Tokens
* **identity token**: proof that a user has been authenticated, encoded as a JWT
* JWT id tokens are an OIDC specification
* id tokens contain three parts: header, payload, signature
* id tokens are **signed** by the token issuer using a private key. using the issuer's public key, you can verify the token is valid
* the identity token payload contains **claims**: key-value pairs containing information about user and the token itself
```json
// payload
{
	"iss": "http://my-domain.auth0.com", 	// issuer
	"sub": "auth0|123456", 					// subject (user)
	"aud": "1234abcdef", 					// audience
	"exp": 1311281970,						// expiration
	"iat": 1311280970,						// issued at
	"name": "Jane Doe",						// user claims...
	"given_name": "Jane",
	"family_name": "Doe"
}
```
* OIDC spec does not require that the id token contain info about the user -- just info about the authentication operation itself, but they typically do
* user claims may include things like name, email, picture url, etc
* **audience** (`aud`) is the client id of the web app intended to be the final recipient of the token
* what can you do with an id token?
	- claims about the user's identity can be trusted (assume the user is authenticated)
	- personalize the user experience (show their profile picture) without making additional requests
* what should you not do with an id token?
	- call an API or check if the client is authorized to access a resource
	- if you're working in a first-party scenario (you own the client and the API) you may be tempted to use an id token for authorization
	- if an attacker gets ahold of the id token, they can use it to impersonate the user and call your API as though they're the user
	- additionally, id tokens do not contain scopes, and the audience of the id token is the client not the resource, so allowing id tokens to be used to call an API would introduce security holes

### Access Tokens
* access tokens are an OAuth specification
* an access token allows one application to access protected resources from another service on behalf of the user
* the client app may also perform specific actions on behalf of the user
* this is known as a **delegated authorization** scenario
* the **client app** is the app that needs to access the protected resources
* the user is the **resource owner**
* the service that hosts the protected resources is the **resource server**
* the **authorization server** provides access tokens to clients
* **scopes** are used to define the access permissions granted to the client app: e.g. an app may be authorized to cross-post to Twitter but it may not delete existing posts or edit your profile
* a **user-centric access token** (used in authorization code flow) will contain data about the user, as it is the user who is authorized to access the resource
* **sender constraint** bind an access token to a specific client app -- even if an attacker steals the access token, they cannot use it
* the resource server **validates** the access token and grants access to the protected resources
* access tokens are **not** meant to be understood by the client app -- they are meant to be sent to the resource server
* OAuth does not specify the format or contents of the access token -- it can be a string in any format, but JWTs are commonly used and it may take a shape very similar to id token JWTs with a header, payload and signature with scopes instead of claims
* thus *understanding how to retrieve and interpret information to make authorization decisions is an agreement between the authorization server and the resource server*. the client is just a dumb pass-through and should treat the access token as an opaque string
	- however the request response that retrieves the token will typically include information for the convenience of the client such as expiration and scopes
* **validation rules** for access tokens (what the resource server should be checking) -- this is typically handled by a library
	- the signature is valid and algorithm is followed
	- `typ` must be `at+jwt`
	- `iss` must be an expected value
	- current time must be before `exp` and after `iat`
	- `scope` must contain an expected value
	- verify the client has permission to access the resource, and if the token is user-centric, verify the user has permission as well
* **TODO** what are some examples of access token format/info and how are they used?
* what can you do with an access token?
	- a client application can access the user's protected resources and take actions on behalf of the user
	- the resource server can verify if the client is allowed to access something
	- for example: you may authorize your LinkedIn account to publish your posts on Twitter
* what shouldn't you do with an access token?
	- you should not attempt to interpret or use the access token in the client application -- the token is between the resource server and the authorization server
	- the maintainers of the auth/resource servers have no obligation to tell you if they change the format, so if you try to access the token in your app, it could break


### Refresh Tokens
* for user-centric access tokens (and other tokens?), we don't want to redirect the user to the auth server every time the token expires, so we use a **refresh token** to get a new access token
* refresh tokens are **long-lived**, however they can be **revoked**: the user may go to the resource server and "disconnect" third party apps -- the next time the app tries to access the resource, it will find the refresh token is no longer valid
* there is also a programmatic way to revoke refresh tokens: the client app can call the auth server and revoke the refresh token when the user logs out of their system


### Token Management
* when your token fails, you should get a 401 vs 403: they're both authorization issues and they require different action to be taken
	- 401 (unauthorized)
		* you're trying to access a resource, but the token is expired, isn't valid or is missing
		* very common to have an expired token -- get a fresh token and retry 
	- 403 (forbidden)
		* you're trying to access a resource and your token is valid, but you don't have permission to access it
		* nothing you can do about this -- surface the error to the user so they can request access or stop trying to do what they're not allowed to do
* you don't want to get a new token every time you make a request
* rather tokens should be stored server-side: in memory, distributed cache, cookie, session storage, database, etc
* two approaches to refreshing tokens: 
	- **re-active**: use the stored token until you get a 401 and then refresh it/restore the fresh token
	- **pro-active**: keep track of expiration and renew ahead of time
* access tokens are short-lived, and for user-centric access tokens we don't want to go through the whole process of redirecting the user to the auth server every time the token expires
	- instead, we use a **refresh token** to get a new access token


## SAML
* SAML is an authentication protocol: an XML-based open standard for exchanging identity info between services
* when a browser contacts a web app, SAML 2.0 is common
* "SAML is the Windows XP of Identity"
* SAML uses **SAML assertions**: a cryptographically signed XML document containing information about the user and what the user can access
* it is heavy-handed, not a simple protocol to implement -- commonly used in work environments

### SAML SSO Workflow
sample flow: you want to log in to your company's gmail account using your work credentials

1) **service provider login**: you visit gmail.com (gmail is the **service provider**) and click SSO login
2) **service provider returns SAR to browser**: gmail sees the email is from your work domain so it sends a SAML auth request back to the browser to tell it "I can't auth this, go ask the IDP"
3) **browser forwards SAR to IDP**: the browser redirects to the **IDP** specified in the auth request. common IDPs for this scenario include Okta, OneLogin, and Auth0
4) **display IDP login page**: you see a login prompt for your work account
5) **send login credentials**: you log in, sending cred to IDP
6) **return signed SAML assertion**: the IDP generates a SAML response and sends it to the browser
7) **browser sends SAML assertion to service provider**: the SAML response is forwarded back to gmail
8) **service provider validates SAML assertion**: gmail validates that the assertion was signed by the IDP, usually done with pub key cryptography
9) **service provider grants access based on SAML access permissions**: you finally get to check your email

What happens when the user tries to access another SSO-integration application, like Slack? The same process as above but when the redirect to IDP occurs, the user has already logged in so login is skipped and a SAML assertion is generated for Slack containing user info and access data and the process proceeds as before -- essentially we skip steps 4 and 5

## Windows Auth

### Troubleshooting Windows Auth
* you can view the logs generated from login attempts:
	- event viewer -> windows logs -> security
	- there you can search by name of the user
	- look for event ids: 4625 error message




* client_id is for the app, not user



