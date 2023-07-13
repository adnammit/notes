# Authentication

## Overview
* in ye olden days (and for some applications presently) [**Windows Authentication**](https://docs.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview) (aka NTLM or kerberos) is used
	- the client browser displays a login dialog box to capture username and password
	- the client sends the encoded (note: *not* encrypted) credentials back to the server where IIS directly handles authentication
	- Windows Auth is well-suited to intranet environments:
		* client and server are in the same domain
		* http proxy connections are not required
	- [more info here](https://www.c-sharpcorner.com/UploadFile/84c85b/understanding-windows-authentication-in-detail/)
* modern authentication tends to be handled with some flavor of **OIDC**

### Troubleshooting Windows Auth
* you can view the logs generated from login attempts:
	- event viewer -> windows logs -> security
	- there you can search by name of the user
	- look for event ids: 4625 error message

## Single Sign-On (SSO), SAML, OAuth and OIDC
* SSO allows users to access many applications using the same identity
* we have an app we need to access, like YouTube -- this is the **service provider**
* and we have a separate service which manages the user identity -- this is the **identity provider** or IDP
* SSO uses the concept of **federated identity** which allows sharing of identity information across trusted but independent systems
* SSO can be implemented using SAML or OAuth (OIDC), or both
	- OAuth is an authorization process
	- SAML and OIDC are both standards for federated authentication
* **security tokens**
	- **access token**
	- **identity token**
* when talking to an API, we tend to use **OAuth2**
	- OAuth2 is not actually an authentication protocol -- it's just for securely passing access tokens (especially from clients which are inherently untrustworthy)
	- _OAuth2 by itself should only be used to get info from an API, it should never be used to sign into a client application_ -- it is not for authorization
* distinguishing **identity** and **access**
	- IdentityServer and Azure AD implement the OAuth2 standard -- they are **identity providers** (IDPs)
	- this is as opposed to **authorization servers**
	- more and more, even if you just need an access token, you're probably working with OIDC
* [OIDC, OAuth and SAML](https://www.okta.com/identity-101/whats-the-difference-between-oauth-openid-connect-and-saml/)

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
		* the user proves who they are (by creating a user/pw combo)
	- the IDP creates an id token and signs it
	- the user is redirected back to the client application
		* the client application receives the identity token and validates it
		* if the validation checks out, the client now has proof of identity
		* if the client is an ASP.NET app, a claims identity is created
			- stored as an authentication ticket in an encrypted cookie
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

### OIDC or SAML SSO?
* many IDPs provide support for both, including Duo, JumpCloud, Okta, MS and onelogin
* boils down to which is easier to integrate with your application
* if you're creating a new web app and are integrating with other OIDC providers, OIDC is probably the way to go

**START HERE** https://vimeo.com/113604459 12:42


* client_id is for the app, not user
* adfs.altsrc.net should be whitelisted
* access to Auth anywhere in app
	- set axios default header to include header w/ token in every post



