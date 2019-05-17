# Bookings

## List of bookings

After receiving and pairing the apartments, the Client should request 
the list of the existing bookings for each apartment.

### Endpoint
```
GET /api/bookings/{apartment_number}
```
### Parameters
- `apartment_number` (integer, required)

### Response 200 (application/json)

Field | Type | Description
------|------|------------
apartment_number | integer | ID of the apartment entity on apartment.at
booking_number | string | ID of the booking entity on apartment.at
status | string | The status of the booking (`"open"`, `"finished"` or `"cancelled"`)
arrival | string | The day of the check-in in `YYYY-MM-DD` format
departure | string | The day of the check-out in `YYYY-MM-DD` format
first_name | string | First name of the guest
last_name | string | Last name of the guest
nationality | string | Nationality of the guest
email | string | E-mail address of the guest
phone | string | Phone number of the guest
language | string | The preferred language of the guest (`"de"` or `"en"`)
adults_number | integer | Number of adults
children_number | integer | Number of children
infant_number | integer | Number of infants
booking_type | string | Type of the booking (`"inquiry"`, `"instant"` or `"booking_on_request"`)
paid | number | The amount of the price the guest already paid (can be decimal)
remaining | number | The amount of the price the guest has to pay on location (can be decimal)

#### Services
There are 3 types of services: inclusive, compulsory and optional. All three service types are sent with 
each response, regardless if they contain any services or not.

Field | Type | Description
------|------|------------
name | string | Name of the service
price | number | Price of the service, `null` if the service is inclusive
frequency | string | The frequency of charging for the service (`"one_off"`, `"per_person"`, `"per_night"`, `"per_person_per_night"`, `"per_booked_unit"` or `"per_booked_unit_per_night"`)
quantity | integer | The amount of the service the price is being applied to
total | number | The price multiplied by the amount of the service, not available if the service is inclusive

```json

{
  "bookings": [
    {
      "apartment_number": 123456,
      "booking_number": "ABC12345678",
      "status": "open",
      "arrival": "2018-01-01",
      "departure": "2018-01-10",
      "guest": {
          "first_name": "First",
          "last_name": "Last",
          "nationality": "british",
          "email": "test@test.com",
          "phone": "+4312345678",
          "language": "en"
      },
      "adults": 4,
      "children": 0,
      "infants": 0,
      "booking_type": "inquiry",
      "prices": {
          "services": 
              {
                "inclusive": [
                  {
                    "name": "Internet (WiFi)",
                    "price": null,
                    "frequency": "per_night",
                    "quantity": 1
                  },
                  {
                    "name": "Final cleaning",
                    "price": null,
                    "frequency": "one_off",
                    "quantity": 1
                  },
                  {
                    "name": "Towels",
                    "price": null,
                    "frequency": "per_person",
                    "quantity": 1
                  },
                  {
                    "name": "Bed linen",
                    "price": null,
                    "frequency": "per_person",
                    "quantity": 1
                  }
                ],
                "compulsory": [
                  {
                    "name": "Local tax",
                    "price": 7.7,
                    "frequency": "one_off",
                    "quantity": 1,
                    "total": 7.7
                  }
                ],
                "optional": [
                  {
                    "name": "Garage",
                    "price": 30,
                    "frequency": "per_booked_unit_per_night",
                    "quantity": 1,
                    "total": 30
                  },
                  {
                    "name": "Baby Bed",
                    "price": 10,
                    "frequency": "per_night",
                    "quantity": 1,
                    "total": 10
                  },
                  {
                    "name": "Animals",
                    "price": 10,
                    "frequency": "per_night",
                    "quantity": 2,
                    "total": 20
                  }
                ]
              }
          ,
          "paid": 400,
          "remaining": 100
      }
    }
  ]
}

```

## New booking

When a new booking is created, apartment.at will send a notification to the Client 
if a callback URL was provided with the subscription. 

To receive booking data, the Client needs to call [Get booking](#get-single-booking).

In case of an error, the notification will be sent again in 3 minute intervals up to **three times** until the proper 
status code is returned. After that, apartment.at will notify the host 
about the failed synchronization. The same applies to booking update notifications.

### Request POST (application/json)

Field | Type | Description
------|------|------------
timestamp | string | The date and time of the request in full ISO 8601 format (UTC)
event | string | The type of the event (`"new"`)
apartment_number | integer | ID of the apartment entity on apartment.at
booking_number | string | ID of the booking entity on apartment.at

```json
{
    "timestamp": "2018-05-04T12:14:00+00:00",
    "event": "new",
    "booking": {
        "apartment_number": 123456,
        "booking_number": "ABC12345678"
    }
}
```

### Response 204

## Booking updated

When a change happens to a booking on apartment.at, a notification will be sent to the Client.

To receive booking data, the Client needs to call [Get booking](#get-single-booking). 

The Client will receive all booking data, not just the updated values.

### Request POST (application/json)

Field | Type | Description
------|------|------------
timestamp | string | The date and time of the request in full ISO 8601 format (UTC)
event | string | The type of the event (`"update"`)
apartment_number | integer | ID of the apartment entity on apartment.at
booking_number | string | ID of the booking entity on apartment.at

```json
{
    "timestamp": "2018-05-04T12:14:00+00:00",
    "event": "update",
    "booking": {
        "apartment_number": 123456,
        "booking_number": "ABC12345678"
    }
}
```

### Response 204

## Get single booking

Get all data from a specific booking.

### Endpoint
```
GET /api/booking/{booking_number}
```
### Parameters
- `booking_number` (required)

### Response 200 (application/json)

Field | Type | Description
------|------|------------
apartment_number | integer | ID of the apartment entity on apartment.at
booking_number | string | ID of the booking entity on apartment.at
status | string | The status of the booking (`"open"`, `"finished"` or `"cancelled"`)
arrival | string | The day of the check-in in `YYYY-MM-DD` format
departure | string | The day of the check-out in `YYYY-MM-DD` format
first_name | string | First name of the guest
last_name | string | Last name of the guest
nationality | string | Nationality of the guest
email | string | E-mail address of the guest
phone | string | Phone number of the guest
language | string | The preferred language of the guest (`"de"` or `"en"`)
adults_number | integer | Number of adults
children_number | integer | Number of children
infant_number | integer | Number of infants
booking_type | string | Type of the booking (`"inquiry"`, `"instant"` or `"booking_on_request"`)
paid | number | The amount of the price the guest already paid
remaining | number | The amount of the price the guest has to pay on location

#### Services
There are 3 types of services: inclusive, compulsory and optional. All three service types are sent with 
each response, regardless if they contain any services or not.

Field | Type | Description
------|------|------------
name | string | Name of the service
price | number | Price of the service, `null` if the service is inclusive
frequency | string | The frequency of charging for the service (`"one_off"`, `"per_person"`, `"per_night"`, `"per_person_per_night"`, `"per_booked_unit"` or `"per_booked_unit_per_night"`)
quantity | integer | The amount of the service the price is being applied to
total | number | The price multiplied by the amount of the service, not available if the service is inclusive

```json
{
    "booking": {
        "apartment_number": 123456,
        "booking_number": "ABC12345678",
        "status": "open",
        "arrival": "2018-01-01",
        "departure": "2018-01-10",
        "guest": {
            "first_name": "First name",
            "last_name": "Last name",
            "nationality": "english",
            "email": "test@test.com",
            "phone": "+4312345678",
            "language": "en"
        },
        "adults_number": 4,
        "children_number": 0,
        "infant_number": 0,
        "booking_type": "inquiry",
        "prices": {
            "services": 
                {
                  "inclusive": [
                    {
                      "name": "Internet (WiFi)",
                      "price": null,
                      "frequency": "per_night",
                      "quantity": 1
                    },
                    {
                      "name": "Final cleaning",
                      "price": null,
                      "frequency": "one_off",
                      "quantity": 1
                    },
                    {
                      "name": "Towels",
                      "price": null,
                      "frequency": "per_person",
                      "quantity": 1
                    },
                    {
                      "name": "Bed linen",
                      "price": null,
                      "frequency": "per_person",
                      "quantity": 1
                    }
                  ],
                  "compulsory": [
                    {
                      "name": "Local tax",
                      "price": 7.7,
                      "frequency": "one_off",
                      "quantity": 1,
                      "total": 7.7
                    }
                  ],
                  "optional": [
                    {
                      "name": "Garage",
                      "price": 30,
                      "frequency": "per_booked_unit_per_night",
                      "quantity": 1,
                      "total": 30
                    },
                    {
                      "name": "Baby Bed",
                      "price": 10,
                      "frequency": "per_night",
                      "quantity": 1,
                      "total": 10
                    },
                    {
                      "name": "Animals",
                      "price": 10,
                      "frequency": "per_night",
                      "quantity": 2,
                      "total": 20
                    }
                  ]
                }
            ,
            "paid": 400,
            "remaining": 100
        }
    }
}
```