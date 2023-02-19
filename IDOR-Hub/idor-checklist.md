# IDOR

- [ ]  Find and Replace 10s  in urls, headers and body: /users/01 → /users/02
- [ ]  Try Parameter Pollution: users-01 users-01&users-02
- [ ]  Special Characters: `/users/01` of `/users/*` → Disclosure of every single user
- [ ]  Try Older versions of api endpoints: `/api/v3/users/01` → `/api/v1/users/02`
- [ ]  Add extension: `/users/01` → `/users/82.json`
- [ ]  Change Request Methods: `POST /users/81` → `GET, PUT, PATCH, DELETE` etc
- [ ]  Check if `Referer` or some other `Headers` are used to validate the `IDs`:
`GET /users/02` → `403 Forbidden
Referer: [example.com/users/01](http://example.com/users/01)
GET /users/82` → `200 OK
Referer: [example.com/users/02](http://example.com/users/02)`
- [ ]  Encrypted IDs: If application is using encrypted IDs, try to decrypt using [hashes.com](http://hashes.com/) or other tools.
- [ ]  Swap GUID with Numeric ID or email:
`/users/1b84c196-89f4-4260-b18b-ed85924ce283` or `/users/82` or `/users/agb.com`
- [ ]  Try GUIDs such as:
`00000000-0000-0000-0000-000000000000` and `11111111-1111-1111-1111-111111111111`
- [ ]  GUID Enumeration: Try to disclose GUIDs using `Google Dorks`, `Github`, `Wayback`, `Burp history`
- [ ]  If none of the GUID Enumeration methods work then try: `Signup`, `Reset Password`, Other endpoints within application and analyze response. These endpoints mostly disclose user's GUID.
- [ ]  `403/401` Bypass: If server responds back with a `403/401` then try to use burp intruder and
send `50-100` requests having different IDs: Example: from `/users/01` to `/users/100`
- [ ]  if server responds with a `403/401`, double check the function within the application.
Sometime `403/401` is thrown but the action is performed.
- [ ]  Blind IDORS: Sometimes information is not directly disclosed. Lookout for endpoints and
features that may disclose information such as `export files`, `emails` or `message alerts`.
- [ ]  Chain `IDOR` with `XSS` for `Account Takeovers`.
