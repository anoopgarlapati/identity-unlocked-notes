# JWT Profile for OAuth2 Access Tokens

[Podcast](https://identityunlocked.auth0.com/public/49/Identity%2C-Unlocked.--bed7fada/7efd0ecb)

[JSON Web Token (JWT) Profile for OAuth 2.0 Access Tokens](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-access-token-jwt)

What is JWT Profile for OAuth2 Access Tokens?
- if you're using JWT as format for access tokens, this spec provides set of guidelines on standard claims to include in JWTs, signature and validation of JWTs
- the resource server can easily look up JWT access tokens over opaque tokens which require additional introspection calls to lookup details
- industry was already using JWTs for ID token

Learning how to write a draft specification:
- first create an individual draft and upload it to IETF 
- IETF working groups after several discussions would vote to take it under the working group
- help from community members in the IETF working group on how to write spec in XML and Markdown

Debate on the spec:
- whether to restrict the spec to single resource
- how to differentiate JWT access tokens and ID tokens
- the propositions on mailing list really enhance the draft

Benefits of JWT profile for access tokens:
- interoperability with generic SDK 
- stops misuse of ID tokens instead of access tokens
  - ID token audience is only the client and not supposed to be passed to resource server
  - ID tokens are not designed to be sender constrained as the audience is only the client
  - so using JWT format for access token would achieve above two capabilities
- kubernetes for example need a JWT and use ID token and presents some challenges

