## AUTHENTICATION
* **security tokens**
    - **access token**
    - **identity token**
* when talking to an API, we tend to use **OAuth2**
    - OAuth2 is not actually an authentication protocol -- it's just for securely passing access tokens (especially from clients which are inherently untrustworthy)
    - _OAuth2 by itself should only be used to get info from an API, it should never be used to sign into a client application_ -- it is not for authorization
* when a browser contacts a web app, SAML 2.0 is common
    - "SAML is the Windows XP of Identity"
    - it is heavy-handed, not a simple protocol to implement
* so neither of these things is ideal - what do we do?
    - **OpenID Connect** (OIDC)
        * not really that simple to implement either
        * it's an identity layer on top of OAuth2 -- **OpenId extends OAuth2**
        * defines identity tokens
        * standardizes token type -- makes everything more inter-connectable
        * defines standard cryptography
        * defines token validation procedure
        * combines authentication with delegated API access -- since it sits on OAuth2, we have *authentication and access control in a single protocol*, without needing to learn OAuth2 -- just implement OpenID
        * also because it is combined with OAuth2, there's only one trip to the server
* distinguishing **identity** and **access**
    - IdentityServer and Azure AD implement the OAuth2 standard -- they are **identity providers** (IDPs)
    - this is as opposed to **authorization servers**
    - more and more, even if you just need an access token, you're probably working with OIDC

## HOW OPENID CONNECT WORKS
* a client application can request an identity token (alongside an access token if needed)
* the identity token is used to sign in to the client application
* that same access token can be used to access an API
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

### HOW IS THIS SECURE?
* all of this transfer of sensitive data is handled over **TLS (SSL)**             
* the redirect to the provider should take place in a new window (rather than being embedded in the web app) so the app cannot snoop on the user's credentials


### OIDC ENDPOINTS
* authorization endpoint (IDP level)
    - used by the client app to obtain access/identity tokens via redirection
* redirection endpoint (client level)
    - the client endpoint that the user is redirected to after receiving the code/token(s) from the IDP
* token endpoint (IDP level)
    - clients can programatically request tokens from the IDP via HTTP POST without redirection

### OIDC FLOWS
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



**START HERE** https://vimeo.com/113604459 12:42


* client_id is for the app, not user
* adfs.altsrc.net should be whitelisted
* access to Auth anywhere in app
    - set axios default header to include header w/ token in every post
