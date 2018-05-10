# Apartment.at API

Current version: **v1**

## Description

An API for apartment.at to synchronize apartments, bookings, availability, minimum stay and prices with third-party channel managers. Functionality relies on the implementation of this API by the third-party site (Client).

## Getting Started

Activating the API is done through these steps:

1. The host logs in to the channel manager and activates "apartment.at" channel
2. The host is redirected to apartment.at. If he is not logged in, the system asks for his credentials.
3. The host is asked to enable access to the channel manager.
4. The channel manager links the apartments by subscribing to them.
5. The channel manager receives the apartment's bookings from apartment.at.
6. The channel manager sends availability data to apartment.at.