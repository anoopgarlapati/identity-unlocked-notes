# OAuth2 Sender Constraint Support: DPoP and mTLS

[Podcast](https://identityunlocked.auth0.com/public/49/Identity%2C-Unlocked.--bed7fada/4d4e8add)

[RFC 8705 - OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens Tokens](https://datatracker.ietf.org/doc/html/rfc8705)
[OAuth 2.0 Demonstrating Proof-of-Possession at the Application Layer (DPoP)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-dpop)

What problem is sender constraint trying to solve?
- in oauth the tokens that are used are bearer tokens
- whoever has the bearer token can access resources with it
- if bearer token falls in wrong hands then it compromises the entire system
- sender constraint adds additional layer of security by constraining the bearer token to the system (client) that is supposed to use it
- typically issued by authorization server with some kind of binding between token and a public key
- client has to demonstrate possession of corresponding private key when using the token

Current standards with sender constraint:
- currently two standards prevalent in oauth space
- mTLS RFC 8705
- DPoP

mTLS:
- robust and stable
- binds access token to client certificate used in mutually authenticated TLS connection over HTTPS

DPoP:
- demonstrating proof-of-possession at HTTP application layer
- easier to use than mTLS
- client creates little JWT with its public key and sends it in each request (to authorization server or resource server) in HTTP header
- authorization server needs to verify the JWT and include a hash in token that binds to the client's public key
- resource server needs to do normal validation and validate the client's public key and DPoP hash

mTLS vs DPoP:
- mTLS is an RFC, DPoP is an IETF working draft
- mTLS has challenges in deployment
- DPoP is a draft so might things might change, so implementations should be prepared to change
- DPoP is in a good shape
- Token Exchange, mTLS have gone into deployment in some implementations before becoming RFCs
- DPoP can have a phased rollout between authorization server, client and resource server components

What about binding refresh tokens?
- Usually refresh token is bound to client and if the client is confidential client so it is effectively sender constrained to the client authentication method
- Refresh tokens issued to public clients do not have sender constraint
- DPoP offers binding of refresh tokens to DPoP public key for public clients