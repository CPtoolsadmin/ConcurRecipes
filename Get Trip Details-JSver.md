## API Recipes: API Tasks
This series of API Recipes describes API tasks associated with **TripLink** Suppliers who need to get data about their employees’ travel related booking.  This recipe assumes you are a current Car or Hotel TripLink Supplier customer.

#### Before you begin
Review the following checklist to ensure you are able to perform the task in this recipe. To see reference information, click the link in the bullet point.

- Understand the [Oath 2.0 process](https://developer.concur.com/api-reference/authentication/authentication.html)
- Ensure you have a definition ([XSD](https://developer.concur.com/api-reference/travel/itinerary/ItinServices_Public_0.xsd)), usable for architecting your solution

#### Get Trip Details: Pulling Trip data from Concur
The Get Trip endpoint allows TripLink Supplier developers to pull trip detail data from Concur to proactively offer services for an upcoming trip.  A TripLink car supplier can get upcoming trips for a traveler to offer a rental car if their trip lacks a rental vehicle registration. A TripLink Hotel supplier can get upcoming trips for a traveler in order to offer hotel accommodations if their trip lacks a hotel reservation.

##### Get Trip details:

Triplink Suppliers may either build a scheduled process to poll for itineraries or get trips on demand when a user logs onto your website to make a booking. Using the Oauth token of the user for whom you want details as the authorization, perform this two-step process:


1. GET a list of trips for the user.
2. GET the details of each trip one at a time.

Consult the article on how to [GET a trip list](https://developer.concur.com/api-reference/travel/itinerary/trip/trip-resource.html#getts) for a user by using their Oauth token for more information.

##### Example Get List of Trips request:
You can use a scheduled job to poll at an interval or a process to get trips when the traveler logs into the TripLink Partner’s website or mobile app to make a booking. Use either of the following queries.
  ```
  GET https://{InstanceURL}/api/travel/trip/v1.1 HTTP/1.1
  Authorization: OAuth {user’s access token}
  Accept: application/xml
  ```
Or
```
GET /travel/trip/v1.1/{query_parameters}
```
This table lists and describes the Query parameters available for GET trip summaries

Name | Type | Data Type | Description
------- | ----- | ----------- | -------------
startDate | date | date Time | The URL-encoded start date (in Coordinated Universal Time, aka UTC) for the trip. Format: YYYY-MM-DD. If no query parameters are provided, the start date is set to today’s date - 30 days.
endDate | date | date Time | The URL-encoded UTC end date for the trip. Format: YYYY-MM-DD. If no query parameters are provided, the end date is set to today’s date + 12 months.
createdAfterDate | date | date Time | The URL-encoded UTC date for when the trip was created. The query string will return trips created on or after this date. Used with the createdbeforedate for finding trips created during a date range. Format: YYYY-MM-DD.
createdBeforeDate | date | date Time | The URL-encoded UTC date for when the trip was created. The query string will return trips created on or before this date. Used with the createdafterdate for finding trips created during a date range. Format: YYYY-MM-DD.
lastModifiedDate | date | date Time | The last modified UTC date of the trips and any their associated bookings. This query string will return only the trips where the trip or any of its associated bookings have a last modified date that is greater or equal to the supplied time.
bookingType | date | string | The trip includes at least one booking of this type. Format: Air, Car, Dining, Hotel, Parking, Rail, or Ride.
userid_type=login | type | string | The loginID is the user’s Concur login ID.This parameter can only be used if the OAuth consumer has one of the user roles listed above.
userid_value | userid | string | The userid_value of ALL can be sent to get trip summaries for all users at the company. This parameter can only be used if the OAuth consumer has one of the user roles listed above.
includeMetadata | true/false | string | The includeMetadata query parameter combined with the ItemsPerPage and Page query parameters cause the response to be divided into pages. The response is wrapped in a ConcurResponse parent element, with both the response details and the paging metadata included.
ItemsPerPage | number | integer | The includeMetadata query parameter combined with the ItemsPerPage and Page query parameters will cause the response to be divided into pages. The response will be wrapped in a ConcurResponse parent element, with both the response details and the paging metadata included.
includeVirtualTrip | flag | integer | Virtual trips are segments booked offline through the Travel Request product. Set the includeVirtualTrip query parameter to 1 to include those trips in the list.
includeCanceledTrips | true/false | string | The includeCanceledTrips query parameter will cause the request to also return trips with a status of Canceled. When this query parameter is set to true, the response will include the TripStatus element

Refer to this sample [Get trip summaries by start and end date]( https://developer.concur.com/api-reference/travel/itinerary/trip/trip-resource.html#getts) response for guidance.

#### Get the details of each trip by using the TripID.

The Get Trip List response includes the unique TripID for each trip. Get the details of each trip, one at a time, using the unique TripID of each trip from the summary.

Refer to this sample [Get trip details query]( https://developer.concur.com/api-reference/travel/itinerary/trip/trip-resource.html#getts) for guidance.

##### Important
No itinerary can contain all of the possible elements, because the response will always be a subset of the possible returned values. For full response details, view the [Public Itinerary XSD](https://developer.concur.com/sites/default/files/ItinServices_Public_0.xsd ).

Once you have the trip details, look for trips that lack your segment type.
 For example:
 - If you are a TripLink Car Supplier, look for trips that may include a flight but which lack a rental car reservation.

 - If you are a TripLink Hotel Supplier, look for trips that may include a flight but which lack a hotel reservation.

 - If you are polling for trips at an interval, you may choose to email a traveler to offer them your service to add to their upcoming trip.  For example, if you are a TripLink Hotel partner, you may choose to email the traveler: “If you would like to reserve a hotel for your upcoming trip to Boston on 13 December, we are offering a special rate at our property in city center.  Log on here to add a hotel reservation to your trip.”  

 - If you are checking for upcoming trips when the traveler logs in to your mobile app or website to make a booking, you may choose to present to the traveler a list of their upcoming trips that lack your segment type.  For example, if you are a TripLink Car Supplier, you may choose to present a list of upcoming trips that lack a rental car reservation and allow the traveler to easily add a car rental to that trip, prepopulating the pickup and drop-off locations and dates/times using the information from the trip details.

#### Make us better at making your experience easier.
Share a Concur API process issue we can do better. Provide us with an explanation, screen shots and your recommendation [here](http://forum.developer.concur.com/).
