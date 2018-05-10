# Apartment functions

## List of apartments

The Client can retrieve all apartments that belong to the user. The linking of the apartments between the Client Application and the Resource Server can be implemented several ways. The users can input the corresponding IDs for each apartment or the Client Application can provide a GUI to pair them.

### Endpoint
```
GET /api/apartments
```

### Response 200 (application/json)

Field | Type | Description
------|------|------------
apartment_number | integer | ID of the apartment entity on apartment.at
name | string | Name of the apartment in english and german
postal_code | integer | Postal code in 4 digit format
city | string | City of the apartment, defaults to "Wien"
street | string | Street name and building number
stair | string | Stairway number *(optional)*
floor | string | Floor number *(optional)*
door | string | Door number *(optional)*

```
{   
    "apartments": [
        {
            "apartment_number": 123456,
            "name": {
                "de": "Apartment mit Balkon",
                "en": "Apartment with balcony"
            },
            "address": {
                "postal_code": 1010,
                "city": "Wien",
                "street": "Baumgasse 1.",
                "stair": "",
                "floor": "",
                "door": ""
            }
        },
        {
            "apartment_number": 123457,
            "name": {
                "de": "Apartment Blau",
                "en": "Apartment Blue"
            },
            "address": {
                "postal_code": 1012,
                "city": "Wien",
                "street": "Baumgasse 1.",
                "stair": "",
                "floor": "",
                "door": ""
            }
        }
    ]
}
```

## Subscribe to the apartment

In order to start sending data, it is necessary to subscribe to the 
apartment. This way apartment.at will acknowledge that the apartment is 
present in the Client's system.
To receive data from apartment.at, the Client can also include an 
optional callback URL that apartment.at will use to send booking 
notifications to. The URL can be changed by sending a
new request.

Without a subscription apartment.at **WILL NOT** receive or send any data
 between systems, and will always return HTTP 403 status code.

### Endpoint
```
PUT /api/apartments/subscribe
```

### Request PUT (application/json)

Field | Type | Description | Rules
------|------|-------------|------
apartment_number | integer | ID of the apartment entity on apartment.at | required
payload_url | string | The Callback URL apartment.at will use to send data to | 100 characters maximum

```json
{
    "apartment_number": 123456,
    "payload_url": "http://yourdomain.com/endpoint"
}
```

### Response 204

## Unsubscribe from the apartment

The Client can send a request to apartment.at to forget the subscription,
including the callback URL.
It is recommended to implement this feature in case a deletion occurs on 
the Client's side.

### Endpoint
```
PUT /api/apartments/unsubscribe
```

### Request PUT (application/json)

Field | Type | Description | Rules
------|------|-------------|------
apartment_number | integer | ID of the apartment entity on apartment.at | required

```json
{
    "apartment_number": 123456
}
```

### Response 204