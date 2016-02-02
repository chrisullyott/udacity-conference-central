App Engine application for Udacity course `ud858`.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool

## Loading the APIs Explorer

**2016-01-30:** It has been discovered recently by the Udacity FSND community that the APIs explorer may not load in Google Chrome, even after allowing unsecure scripts in the browser. The API seems to load in Mozilla Firefox.

Forum post: [API Explorer Not Showing on Localhost](https://discussions.udacity.com/t/api-explorer-not-showing-on-localhost)

## Written Questions

##### What were your design choices for session and speaker implementation?

_I modeled my API endpoints after the Conference methods, making sure that when I defined Session, it was a child entity of Conference, in the same way that a Conference is a child of the creator (a Profile). I created Sessions as a standalone Entity, and the Session Wishlist as an attribute of the Profile containing the web-safe keys of each Session._

_I set the fields of a Session to require three data points: "name", "typeOfSession", and "speaker", because I felt like a session could not logically exist without knowing these. I set the "duration" field as an integer type in order to accept the number of minutes that the session covers. The websafe entity key for the Session is output via_ `SessionForm` _to make wishlist add and remove requests possible for the app._

##### Think about other types of queries that would be useful for this application. Describe the purpose of two new queries and write the code that would perform them.

_My two extra queries are: "getWishlistSessionsByType" and "getWishlistSessionsBySpeaker" which would allow the user to filter their own wishlist by type of session and also by speaker. This would be helpful for the user when managing their list of conference sessions and helping them to make sure that they will be attending a variety of sessions and seeing a variety of speakers._

##### Let’s say that you don't like workshops, and you don't like sessions after 7pm. How would you handle a query for all non-workshop sessions before 7pm? What is the problem for implementing this query? What ways to solve it did you think of?

_The problem with this query is that there may not be a built-in way in NDB to query for items for a given time of day. We can query for all items that begin before a given date, or after, but querying for a time of day means essentialy handling the same inequality filter for multiple days. That is, we are finding all sessions that begin before 7pm on all days of the conference. To handle this, I would first query for all sessions, excluding the type "workshop", and then with Python, break up the returned sessions into unique lists of sessions per day. Then, I would run a function which removes items from those lists that happen after 7pm on that day. Next, those lists would be combined back into one list to represent all daytime conference sessions._
