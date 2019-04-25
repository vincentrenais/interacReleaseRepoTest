# Proximity SDK

[](Images/Proximity Interactions.png)

## Getting started mobile platform
    [Getting Started iOS](./iOS/Getting Started.md)
    [Getting Started Android](./Android/Getting Started.md)

## Authentication
The Proximity SDK accesses the API Overlay with various secured https requests. A JWT Auth Token passed to the SDK is required to complete these requests. To avoid a double log in scenario, a shared secret has been provided, intended to be used by a financial institution controlled trusted authority to generate authentication tokens. User tokens can be requested from said trusted authority. Do not generated tokens in the application codebase! 


## Token Format

Algorithm: HS256

Its payload must include:

* `iss`: the financial institution's ID, also called `participantId` in the Interac API reference. 
* `sub`: the UUID of the user being authenticated
* `first_name`: the first name of the user, as given by the financial institution
* `last_name`: the last name of the user, as given by the financial institution