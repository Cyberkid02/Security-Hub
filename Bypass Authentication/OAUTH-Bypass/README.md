# OAUTH MISCONFIGURATION

# ⇒ OAUTH MISCONFIGURATION

- Client Application → Web App want to access user's data
- Resource Owner → The user.
- OAuth service provider → application that control user's data and access to it.
- Parameters:
`/?redirect_uri=<Vulnerable.site>&code=<Token>&state=anti_csrf_token`

![image](https://user-images.githubusercontent.com/108616378/219943215-6a48db1b-f10d-47f4-b7ef-008c832dfb4d.png)

## CSRF in OAUTH

![image](https://user-images.githubusercontent.com/108616378/219943230-41cfea35-22be-40e3-9af5-32240afaa35c.png)

### Code in evil.site:
