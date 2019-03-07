# Description

Get the current rates or send new ones to apartment.at whenever a price change occurs in the Client. 
Apartment.at does not send any changes back to the Client.

## Get the rates of every apartment
Retrieve the rates of all the apartments belonging to the user for the next 2 years.

### Endpoint
```
GET /api/prices
```

### Response 200 (application/json)

Field | Type | Description
------|------|-------------
minimum_stay | integer | The minimum number of days the apartment has to be booked for
currency | string | The currency the prices are in
person | integer | The number of persons the prices are referring to
amount | integer | The price of the apartment for the specified amount of guests

```json
{
    "123456": {
        "2019-03-07": {
            "minimum_stay": 3,
            "currency": "EUR",
            "rates": [
                {
                    "person": 1,
                    "amount": 100
                },
                {
                    "person": 2,
                    "amount": 100
                },
                {
                    "person": 3,
                    "amount": 100
                },
                {
                    "person": 4,
                    "amount": 90
                },
                {
                    "person": 5,
                    "amount": 80
                }
            ]
        },
        "2019-03-08": {
            "minimum_stay": 2,
            "currency": "EUR",
            "rates": [
                {
                    "person": 1,
                    "amount": 300
                },
                {
                    "person": 2,
                    "amount": 300
                },
                {
                    "person": 3,
                    "amount": 300
                },
                {
                    "person": 4,
                    "amount": 290
                },
                {
                    "person": 5,
                    "amount": 280
                }
            ]
        }
    },
    "234567": {
        "2019-03-07": {
            "minimum_stay": 1,
            "currency": "EUR",
            "rates": [
                {
                    "person": 1,
                    "amount": 110
                },
                {
                    "person": 2,
                    "amount": 110
                },
                {
                    "person": 3,
                    "amount": 110
                },
                {
                    "person": 4,
                    "amount": 100
                },
                {
                    "person": 5,
                    "amount": 90
                }
            ]
        },
        "2019-03-08": {
            "minimum_stay": 3,
            "currency": "EUR",
            "rates": [
                {
                    "person": 1,
                    "amount": 300
                },
                {
                    "person": 2,
                    "amount": 300
                },
                {
                    "person": 3,
                    "amount": 300
                },
                {
                    "person": 4,
                    "amount": 290
                },
                {
                    "person": 5,
                    "amount": 280
                }
            ]
        }
    }
}
```

## Get the rates of a single apartment
Retrieve the rates of an apartment for the next 2 years.

### Endpoint
```
GET /api/prices/{apartment_number}
```

### Parameters
- `apartment_number` (integer, required)

### Response 200 (application/json)

Field | Type | Description
------|------|-------------
minimum_stay | integer | The minimum number of days the apartment has to be booked for
currency | string | The currency the prices are in
person | integer | The number of persons the prices are referring to
amount | integer | The price of the apartment for the specified amount of guests

```json
{
    "123456": {
        "2019-03-07": {
            "minimum_stay": 3,
            "currency": "EUR",
            "rates": [
                {
                    "person": 1,
                    "amount": 100
                },
                {
                    "person": 2,
                    "amount": 100
                },
                {
                    "person": 3,
                    "amount": 100
                },
                {
                    "person": 4,
                    "amount": 90
                },
                {
                    "person": 5,
                    "amount": 80
                }
            ]
        },
        "2019-03-08": {
            "minimum_stay": 2,
            "currency": "EUR",
            "rates": [
                {
                    "person": 1,
                    "amount": 300
                },
                {
                    "person": 2,
                    "amount": 300
                },
                {
                    "person": 3,
                    "amount": 300
                },
                {
                    "person": 4,
                    "amount": 290
                },
                {
                    "person": 5,
                    "amount": 280
                }
            ]
        }
    }
}
```

## Update prices based on the number of guests

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

## Update prices based on a base price

### Endpoint
```
PUT /api/prices/base/{apartment_number}
```

### Parameters
- `apartment_number` (integer, required)

### Request PUT

Field | Type | Description | Rules
------|------|-------------|------
date | string | The day the prices are referring to | required, `YYYY-MM-DD` format
minimum_stay | integer | The minimum number of days the apartment has to be booked for | required, minimum amount `1`, maximum amount `100`
amount | integer | The price of the apartment | required, minimum amount `1`, maximum amount `1000`
surcharge | integer | The amount of surcharge applied after each additional guest | minimum amount `1`, maximum amount `1000` *(optional)*
persons | integer | The number of persons the price is referring to | required only if the `surcharge` is set, minimum amount `0`, maximum amount `50`. If `0`, the `surcharge` will apply to the entire apartment  *(conditional)*
currency | string | The currency the prices are in | At the moment only `"EUR"` is supported *(optional)*

```json
{
    "prices": [
        {
            "date": "2018-01-01",
            "minimum_stay": 3,
            "amount": 150,
            "currency": "EUR"
        },
        {
            "date": "2018-01-02",
            "minimum_stay": 3,
            "amount": 120,
            "surcharge": 50,
            "persons": 2,
            "currency": "EUR"
        }
    ]
}
```

### Response 204