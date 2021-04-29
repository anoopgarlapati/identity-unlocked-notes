# Whats new in OAuth 2.1

[Podcast](https://identityunlocked.auth0.com/public/49/Identity%2C-Unlocked.--bed7fada/c71b9fbd)

[OAuth 2.1 spec](https://tools.ietf.org/html/draft-ietf-oauth-v2-1-02)

What is OAuth 2.1 about?
- revision to the core OAuth 2.0 spec
- simplifies the spec and make it approachable and easier to understand
- OAuth 2.0 spec consists around 8-12 RFCs worth around 400 pages
- in addition to initial core OAuth 2.0 RFC, there is PKCE, device flow, better cryptographical standards and security recommendations that subsequent specs address
- after the 8-12 RFCs, a simpler picture of OAuth is left that OAuth 2.1 aims to present

New requirement in OAuth 2.1 - PKCE:
- PKCE is added because it is how OAuth is done for mobile apps
- PKCE essentially creates a secret for every authorization request
- authorization code injection attack - even for a confidential client, it is possible to swap the authorization codes with a victim and trick them into logging into your account
- PKCE prevents the above attack as it uses a per request secret

Besides PKCE, what else is added in OAuth 2.1:
- exact matching of redirect URLs
  - prevents open redirect
  - can be relaxed in dev environments
  - can be relaxed in envrionments allowing wildcards for multitenancy but this could be achieved in a different way as well
- refresh tokens should be single use
  - refresh tokens are very powerful if client does not have a secret
  - if attacker attempts to refresh using stolen refresh token, then the authorization server can not only block that refresh token but all access tokens that have been issued to that refresh token
- alternative to above constraint, to solve refresh tokens being powerful is to do some form of sender constraining
  - for mobile apps this can be done with client certificates
  - problem with browser apps is there are no good tools available, could use HTTP only cookies to accomplish something similar
  - this is still in progress and in flux

New defined term - credentialed client:
- goals of OAuth 2.1 is to not define new behavior that doesn't already exist in OAuth 2 world, it is meant to be a consolidation
- confidential clients both have credentials and their identity has been confirmed
- public clients cannot have credentials and use of it does not confirm an app's identity
- credentialed clients are apps which has credentials but does not have its identity confirmed
- credentialed clients surface using dynamic client registration 
- credentialed client is more like a public client that initially is issued a credential through dynamic client registration endpoint and the app uses that credential to prove in subsequent transaction that it is the same client but the client's identity itself is still not confirmed

Deprecations and removals:
- implicit grant and password grant are not included in OAuth 2.1 
- password grant
  - deployments that want password grant are first party deployments that own authorization and the resource servers
  - third party apps should not have access to user's password and that is the point of OAuth 
  - it is not possible to extend password grant to support multifactor authentication methods widely used today
- implicit grant 
  - OAuth 2.1 does not define implicit grant, so only response type option is authorization code
  - OpenID Connect secures the response type token with claims in ID token which the client has to validate to ensure prevention of access token injection attack
  - PKCE solves the same attack but from the authorization server's perspective
  - if clients cannot be trusted to do the right thing then PKCE is the way to go