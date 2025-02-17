# Authentication
- authentication: verifying identity (401 Unauthorized)
- authorization: verifying permissions (403 Forbidden)
- stateful (ie. session using a cookie)
- stateless (ie. token using JWT/OAuth)

## Sessions
### Flow
- user submits login credentials
- server verifies credentials against the DB
- server creates a temp user session
- server issues a cookie with a session ID
- user sends the cookie with each request
- server validates it against the session store and grants access
- logout destroys the session and clears the cookie

### Features
- every user session is stored server-side (stateful)
  - session stored in DB
- each user is identified by a session ID
  - opaque ref (no third party can extract data out)
    - only server can map back to data
  - stored in a cookie
    - signed with a secret (client cannot tamper with the cookie)
    - protected with flags
    
### Cookies
- Cookie is a header, just like authorization or content-type
- cookie consists of name and value
- set with Set-Cookie by server
- cookie is signed with a secret

### CSRF
- unauthorized actions on behalf of the user
- mitigate using CSRF token

### Attributes
- cookie can only be used on a given site and route (Domain and Path)
- Expiration sets expiry
  - when omitted it becomes a session cookie that is deleted when browser is closed

### Flags
HTTPOnly (cannot be read by client side)
Secure (can only be sent over HTTPS channel)

## Tokens
### Flow
- user submits login credentials
- server verifies credentials
- server creates token and embeds user data into it
- server responds back with the token (in body or header)
- ser stores the token in client storage
- user sends the token along each request
- server verifies the token and grants access
- when user logs out, token is cleared

### Features
- tokens are not stored server-side, only to client (stateless)
- signed with secret
- tokens can be opaque or self-contained

### JWT
- open standard
- compact, self-contained, URL-safe tokens

### Client Storage
- JWT can be stored in client storage, local or session
  - local has no expiration time
  - session gets cleared when page is closed

## Sessions vs JWT
### Sessions and Cookies
#### Pros
- Session ID is opaque and carry no meaningful data
- Cookies can be secured with flags
- old-school
#### Cons
- server must store each user in memory
- session auth must be secured against CSRF

### Tokens
#### Pros
- server does not have to keep track of sessions
- any server can verify the token
- CORS is not an issue
#### Cons
- Server still has to maintain a blacklist of revoked tokens
- token is cached and can go stale
- if JWT token is compromised, attacker can really do damage

## OAuth 2
### Overview
- allows users to authenticate themselves using a third-party authentiation service

### Example
- logging into spotify via facebook
- passwords are never passed around

### Workflow
- using spotify and facebook example
- spotify sends an auth request to facebook api
- displays facebook screen asking to authorize spotify to access facebook account information
- authorization grant sent back to spotify along with auth code/access token that spotify will use to access your facebook account
- spotify sends the token to facebook and facebook sends back the protected resource 
- OAuth 2 is the authorization framework
- authentication is done with OpenID Connect using ID tokens

###OpenID Connect
- identity layer on top of OAuth 2 
- allows clints to verify identity based on authentication performed by an auth server as well as to obtain basic profile info abbout the end user


   

