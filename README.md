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

_I set the fields of a Session to require three data points: "name", "typeOfSession", and "speaker", because I felt like a session could not logically exist without knowing these. I set the "duration" field as an integer type in order to accept the number of minutes that the session covers. For "startTime", this would accept an integer as well, in order to accept a Unix timestamp such as_ `1454386900`_._ _The websafe entity key for the Session is output via_ `SessionForm` _to make wishlist add and remove requests possible for the app._

##### Think about other types of queries that would be useful for this application. Describe the purpose of two new queries and write the code that would perform them.

_My two extra queries are: "getWishlistSessionsByType" and "getWishlistSessionsBySpeaker" which would allow the user to filter their own wishlist by type of session and also by speaker. This would be helpful for the user when managing their list of conference sessions and helping them to make sure that they will be attending a variety of sessions and seeing a variety of speakers._

##### Let’s say that you don't like workshops and you don't like sessions after 7 pm. How would you handle a query for all non-workshop sessions before 7 pm? What is the problem for implementing this query? What ways to solve it did you think of?

_To solve this, we would need to query for sessions within the current conference, that begin before 7pm and exclude type: "workshops". The problem with this task is that the current query methods can only pull all sessions of one given type, and cannot return all sessions while excluding a type. We'd need to either write a query that excludes this type of session, or, query for all sessions, and remove these from the returned list. Even though the number of sessions per conference will very likely be less than 100 items, it would still probably be better to query for this and index the query, rather than querying for all sessions and removing them from the returned list in the code. The latter method would not scale as well and be more costly in the long run._

