# Description

In order to connect to the API the Client has to request a client ID and client secret, which is generated and 
sent manually. Our API uses OAuth2 with authorization codes. 
The client application will redirect the user to our server where they will either approve or
 deny the request to issue an access token to the Client.

The base URL used for every request: [https://www.apartment.at/](https://apartment.at/)

The base URL used for testing will be sent along with the API credentials.

## Obtaining authorization code

### Endpoint
```
GET /oauth/authorize?client_id=123&redirect_uri=http://yourdomain.com/redirect_url&response_type=code
```

### Parameters
- `client_id` (required)
- `redirect_uri` (required)
- `response_type` (required) - `"code"`
- `state` (optional) - data that can be used for verification or other purposes by the Client

When receiving authorization requests, the system will display a page to the user, allowing 
them to approve or deny the authorization request. If they approve the request, they will be 
redirected to the `redirect_uri` that was specified by the client application. 
The `redirect_uri` must match the redirect URL that was specified when the client was created.

### Response 200
```
GET response_uri?code=1ba04115153b2dc3ef6f94108d3f7cf3f22b838d24ce6f3152a790e29185662e871ba33d32e847d2
```

## Converting Authorization Codes To Access Tokens

After approving the authorization request, the user will be redirected to the client 
application. The Client should then issue a POST request to the application to request an 
access token. The request should include the authorization code that was issued when the user 
approved the authorization request. 

### Endpoint
```
POST /oauth/token
```

### Request POST
- `client_id` (required) -  the manually provided client ID
- `client_secret` (required) - the manually provided client secret
- `grant_type` (required) - `"authorization_code"`
- `redirect_uri` (required) - must match the redirect URL that was specified when the client was created.
- `code` (required) - the authorization code from the previous request

### Response 200 (application/json)
```json
{
    "token_type": "Bearer",
    "expires_in": 31535998,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUo0Gw (...)",
    "refresh_token": "def502001c929775e9cdba9d5d8cebedc5 (...)"
}
```

## Passing The Access Token

The Client application should specify its access token as a Bearer token in the `Authorization` 
header of every request. The `Accept` header is also required.

### Example header
```
Accept: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUo0Gw (...)
```

## Refreshing The Access Token

Apartment.at issues long-lived access tokens, but they can be renewed regularly if needed.

### Endpoint
```
POST /oauth/token
```

### Request POST
- `client_id` (required) -  the manually provided client ID
- `client_secret` (required) - the manually provided client secret
- `grant_type` (required) - `"refresh_token"`
- `refresh_token` (required) - the refresh token received along with the access token after [converting the authorization code](#converting-authorization-codes-to-access-tokens)

### Response 200 (application/json)
```json
{
    "token_type": "Bearer",
    "expires_in": 31535998,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUo0Gw (...)",
    "refresh_token": "def502001c929775e9cdba9d5d8cebedc5 (...)"
}
```

## Testing the connection

The ping method requests the API to indicate to the Client that it is online and functioning as expected.

### Endpoint
```
GET /api/ping
```

### Response 200 (application/json)
```json
{
    "status": "OK"
}
```

## User data

Information about the apartment.at user associated with the access token.

### Endpoint
```
GET /api/user-data
```

### Response 200 (application/json)
```json
{
    "user": {
        "email": "lorem@ipsum.com"
    }
}
```
