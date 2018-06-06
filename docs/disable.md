Disable the API for the current user.

Once disconnected, all current synchronizations 
will stop, and any request will return HTTP 401 status code. 
To enable it again, a new access token needs to be generated. 


### Endpoint
```
GET /api/disconnect
```

### Response 200

