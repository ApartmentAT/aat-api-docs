
If the host decides to disable the API, all current synchronizations 
will stop. To enable it again, a new API key needs to be generated. 
The host will be prompted if he wants to keep the synchronized bookings, 
or they can be deleted.

Once disconnected, any request will return HTTP 401 status code.

### Endpoint
```
GET /api/disconnect
```

### Response 200

