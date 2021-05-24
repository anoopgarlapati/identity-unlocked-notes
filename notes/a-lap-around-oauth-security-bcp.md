# A lap around the OAuth2 Security BCP

[Podcast](https://identityunlocked.auth0.com/public/49/Identity%2C-Unlocked.--bed7fada/23714281)

[OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics)

What is BCP and how does it differ from core spec?
- a BCP document describes the best current practices for that field
- it gives update on best industry practices but does not override core spec
- provides additional info and practices in OAuth 

Impactful recommendations in the OAuth BCP:
- do not use implicit grant any longer
- if you use the authorization code grant, then you should also use PKCE
- use sender constraining for tokens if possible, otherwise at least use rotation for refresh tokens 

Problems with implicit grant:
- flow where authorization server creates access token and sends it to the browser app that requested the token
- the access token is returned in redirect URL
- they might end up in proxies, browser histories, logs, etc
- token in browser is susceptible to cross site scripting and other problems
- hence use authorization code grant instead

Use authorization code with PKCE:
- originally PKCE was intended just for public clients
- suggestion is to use it everywhere in authorization code grant
- used to protect authorization code from code injection attack for confidential clients
- with PKCE each request has a code challenge and code verifier pair which is essentially a per-request secret
- is nonce from OpenID Connect enough or should I also use PKCE 
  - using PKCE is better as the authorization server controls the security enforcement
  - nonce is checked by the client so authorization server should establish trust with all its clients
  - using PKCE frees up state parameter and state can then be used by clients to store information between requests

Recommendations for sender constrained tokens:
- access tokens and refresh tokens should be bound to the client that is supposed to use the token
- two ways to achieve sender constrained tokens
  - mTLS: client uses certificate to authenticate with authorization server
    - access token and refresh token are bound to the certificate
    - client needs to provide same certificate when using the tokens
  - DPOP: demonstrating proof of possesion
- these may be cryptographically challenging for some clients
- alternate recommendation in such cases is single use or rotation of refresh tokens