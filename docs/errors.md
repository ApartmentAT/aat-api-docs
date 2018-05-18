### 400 Bad request

The request is malformed. Please refer to the relevant section of the 
documentation.

### 401 Unauthorized

The authorization token is missing, invalid or the user has revoked the access.

### 403 Forbidden

The user doesn't have access to the requested resource. Either you trying 
to access a resource belonging to another user or the apartment has no 
active subscriptions.

### 404 Not Found

The requested resource is missing. Please check that you are passing 
the correct `apartment_number`.

### 422 Unprocessable Entity

Check the response body for more information.
```
{
    "messages": "apartment_number is required"
}
```

For more information please refer to the relevant section of the 
documentation.