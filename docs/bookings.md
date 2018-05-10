# Booking functions

## List of bookings

After receiving and pairing the apartments, the Client should request the list of the existing booking for each apartment.

### Endpoint
```
GET /api/bookings/{apartment_number}
```
### Parameters
- `apartment_number` (integer, required)

### Response 200 (application/json)

Field | Type | Description
------|------|------------
apartment_number | integer | ID of the booking entity on apartment.at
booking_number | string | ID of the booking entity on apartment.at
status | string | The status of the booking (`"open"`, `"finished"`, `"cancelled"`)
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
booking_type | string | Type of the booking (`"inquiry"`, `"instant"`, or `"booking_on_request"`)
price_paid | number | The amount of the price the guest already paid (can be decimal)
price_remaining | number | The amount of the price the guest has to pay on location (can be decimal)
currency | string | The currency of the payment, defaults to `"EUR"`

```json
[
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
        "adults_number": 4,
        "children_number": 0,
        "infant_number": 0,
        "booking_type": "inquiry",
        "price_paid": 400,
        "price_remaining": 100,
        "currency": "EUR"
    }
]
```

## New booking

When a new booking is created, apartment.at will send a notification to the Client 
if a callback URL was provided with the subscription. 

To receive booking data, the Client needs to call [Get booking](#get-booking).

In case of an error, the notification will be sent again in 3 minute intervals up to **three times** until the proper 
status code is returned. After that, apartment.at will notify the host 
about the failed synchronization. The same applies to booking update notifications.

### Request POST (application/json)

Field | Type | Description
------|------|------------
timestamp | string | The date and time of the request in full ISO 8601 format
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

To receive booking data, the Client needs to call [Get booking](#get-booking). 

The Client will receive all booking data, not just the updated values.

### Request POST (application/json)

Field | Type | Description
------|------|------------
timestamp | string | The date and time of the request in full ISO 8601 format
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

## Get booking

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
apartment_number | integer | ID of the booking entity on apartment.at
booking_number | string | ID of the booking entity on apartment.at
status | string | The status of the booking (`"open"`, `"finished"`, `"cancelled"`)
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
booking_type | string | Type of the booking (`"inquiry"`, `"instant"`, or `"booking_on_request"`)
price_paid | double | The amount of the price the guest already paid
price_remaining | double | The amount of the price the guest has to pay on location

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
        "price_paid": 400,
        "price_remaining": 100
    }
}
```