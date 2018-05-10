# Availability

## Update availability

The Client has to call this method every time the availability of an 
apartment changes, including every change received via the booking 
webhooks or the booking list request.

### Endpoint
```
PUT /api/availability/{apartment_number}
```

### Parameters
- `apartment_number` (integer, required)

### Request PUT (application/json)

Field | Type | Description | Rules
------|------|-------------|------
date | string | The day the availability is referring to | required, `YYYY-MM-DD` format
space_available | integer | The amount of available space | required, minimum amount is `0`, maximum amount is `50`

```json
[
    {
        "date": "2018-01-01",
        "space_available": 1
    },
    {
        "date": "2018-01-02",
        "space_available": 3
    },
    {
        "date": "2018-01-03",
        "space_available": 0
    }
]
```

### Response 204