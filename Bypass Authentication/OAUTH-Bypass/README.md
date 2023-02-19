# OAUTH MISCONFIGURATION

# ⇒ OAUTH MISCONFIGURATION

- Client Application → Web App want to access user's data
- Resource Owner → The user.
- OAuth service provider → application that control user's data and access to it.
- Parameters:
`/?redirect_uri=<Vulnerable.site>&code=<Token>&state=anti_csrf_token`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/871211ca-9189-44bb-87d2-89f1aed9b150/Untitled.png)

## CSRF in OAUTH

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ac8466e-f891-4c62-b80d-64b186762f73/Untitled.png)

### Code in evil.site:
