---
sidebar_position: 2
---

# Setting up your environment variables

You have a lot of external services to configure in your `.env.local`.

If you want to enable address geolocation, set the variables using [GoogleMap API](https://developers.google.com/maps/documentation/javascript/get-api-key):

```bash
SYMFONY_GOOGLE_MAP_PUBLIC_KEY=INSERT_A_REAL_SECRET
SYMFONY_GOOGLE_MAP_SERVER_KEY=INSERT_A_REAL_SECRET
```

If you want to enable map views, you need a [mapbox](https://www.mapbox.com/) access token with `tokens:read`, `tokens:write` scopes:

```bash
SYMFONY_MAPBOX_SECRET_KEY=sk.xxx
SYMFONY_MAPBOX_PUBLIC_KEY=pk.xxx
```

If you want to enable [recaptcha](https://www.google.com/recaptcha/about/), set the variables:

```bash
SYMFONY_RECAPTCHA_PRIVATE_KEY=INSERT_A_REAL_SECRET
```

If you want to enable sms, set the variables from [Twilio](https://www.twilio.com/):

```bash
SYMFONY_TWILIO_SID=INSERT_A_REAL_SECRET
SYMFONY_TWILIO_TOKEN=INSERT_A_REAL_SECRET
SYMFONY_TWILIO_DEFAULT_SUBACCOUNT_TOKEN=INSERT_A_REAL_SECRET
SYMFONY_TWILIO_DEFAULT_SUBACCOUNT_SID=INSERT_A_REAL_SECRET
SYMFONY_TWILIO_DEFAULT_VERIFY_SERVICE_ID=INSERT_A_REAL_SECRET
```

If you want to enable emails, set the variables from either [Mandrill](https://mailchimp.com/) or [Mailjet](https://www.mailjet.com/):

```bash
SYMFONY_MANDRILL_API_KEY=INSERT_A_REAL_SECRET

SYMFONY_MAILJET_PUBLIC_KEY=INSERT_A_REAL_SECRET
SYMFONY_MAILJET_PRIVATE_KEY=INSERT_A_REAL_SECRET
```

If you want Facebook connect:

```bash
SYMFONY_DEFAULT_FACEBOOK_CLIENT_ID=INSERT_A_REAL_SECRET
SYMFONY_DEFAULT_FACEBOOK_SECRET=INSERT_A_REAL_SECRET
```

If you want France Connect:

```bash
SYMFONY_DEFAULT_FRANCE_CONNECT_CLIENT_ID=INSERT_A_REAL_SECRET
SYMFONY_DEFAULT_FRANCE_CONNECT_CLIENT_SECRET=INSERT_A_REAL_SECRET
```

If you want to configure an Oauth:

```bash
SYMFONY_DEFAULT_OAUTH_CLIENT_ID=INSERT_A_REAL_SECRET
SYMFONY_DEFAULT_OAUTH_CLIENT_SECRET=INSERT_A_REAL_SECRET
# This is an example with keycloak :
SYMFONY_DEFAULT_OAUTH_AUTHORIZATION_URL=https://keycloak.your-domain.com/auth/realms/master/protocol/openid-connect/auth
SYMFONY_DEFAULT_OAUTH_ACCESS_TOKEN_URL=https://keycloak.your-domain.com/auth/realms/master/protocol/openid-connect/token
SYMFONY_DEFAULT_OAUTH_USER_INFO_URL=https://keycloak.your-domain.com/auth/realms/master/protocol/openid-connect/userinfo
SYMFONY_DEFAULT_OAUTH_LOGOUT_URL=https://keycloak.your-domain.com/auth/realms/master/protocol/openid-connect/logout
SYMFONY_DEFAULT_OAUTH_PROFILE_URL=https://keycloak.your-domain.com/auth/realms/master/account
```
