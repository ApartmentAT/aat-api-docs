# Availability

## Update availability

The Client has to call this method every time the availability of an 
apartment changes, including every change received via the booking 
webhooks or the booking list request.

The apartment cannot be booked on apartment.at if no availability data is present.

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
{
    "availability": [
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
    
}
```

### Response 204

## Update CTA/CTD

Send CTA/CTD data to apartment.at.

### Endpoint
```
PUT /api/closed-dates/{apartment_number}
```

### Parameters
- `apartment_number` (integer, required)

### Request PUT (application/json)

Field | Type | Description | Rules
------|------|-------------|------
date | string | The day being updated | required, `YYYY-MM-DD` format
status | string | The chosen status | required, accepted values: <br /> `cta` - closed to arrival, <br /> `ctd` - closed to departure, <br /> `both` - closed to both arrival and departure <br /> `open` - open to both (not closed anymore)

```json
{
    "closed_dates": [
        {
            "date": "2018-06-01",
            "status": "cta"
        },
        {
            "date": "2018-06-15",
            "status": "ctd"
        },
        {
            "date": "2018-06-16",
            "status": "both"
        },
        {
            "date": "2018-06-20",
            "status": "open"
        }
    ]
    
}
```

### Response 204