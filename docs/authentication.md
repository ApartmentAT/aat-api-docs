# Authentication

In order to connect to the API the Client have to request a client id, which is generated and 
sent manually. Our API uses OAuth2 with authorization codes. 
The client application will redirect a user to our server where they will either approve or
 deny the request to issue an access token to the Client.

The base URL used for every request: [https://apartment.at/](https://apartment.at/)

## Obtaining authorization code

### Endpoint
```
GET /oauth/authorize?client_id=123&redirect_uri=http://yourdomain.com/redirect_url&response_type=code
```

### Parameters
- `client_id` (required)
- `redirect_uri` (required)
- `response_type` (required) - `"code"`

When receiving authorization requests, the system will display a page to the user allowing 
them to approve or deny the authorization request. If they approve the request, they will be 
redirected to the `redirect_uri` that was specified by the client application. 
The `redirect_uri` must match the redirect URL that was specified when the client was created.

### Response 200
```
GET response_uri?code=1ba04115153b2dc3ef6f94108d3f7cf3f22b838d24ce6f3152a790e29185662e871ba33d32e847d2
```

## Converting Authorization Codes To Access Tokens

If the user approves the authorization request, they will be redirected back to the client 
application. The Client should then issue a POST request to the application to request an 
access token. The request should include the authorization code that was issued when the user 
approved the authorization request. 

### Endpoint
```
POST /oauth/token
```

### Parameters
- `client_id` (required)
- `client_secret` (required)
- `grant_type` (required) - `"authorization_code"`
- `redirect_uri` (required)
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