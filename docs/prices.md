# Price management

Send the new prices to apartment.at whenever a price change occurs in the Client. 
Apartment.at does not send any changes back to the Client.

## Update prices

### Endpoint
```
PUT /api/prices/{apartment_number}
```

### Parameters
- `apartment_number` (integer, required)

### Request PUT

Field | Type | Description | Rules
------|------|-------------|------
date | string | The day the prices are referring to | required, `YYYY-MM-DD` format
minimum_stay | integer | The minimum number of days the apartment has to be booked for | required, minimum amount `1`, maximum amount `100`
person | integer | The number of persons the prices are referring to | required, minimum amount `1`, maximum amount `50`
amount | integer | The price of the apartment for the specified amount of guests | required, minimum amount `1`, maximum amount `1000`
currency | string | The currency the prices are in | At the moment only `"EUR"` is supported *(optional)*

```json
{
    "prices": [
        {
            "date": "2018-01-01",
            "minimum_stay": 3,
            "rates": [
                {
                    "person": 1,
                    "amount": 100
                },
                {
                    "person": 2,
                    "amount": 130
                },
                {
                    "person": 3,
                    "amount": 150
                }
            ],
            "currency": "EUR"
        }
    ]
}
```

### Response 204