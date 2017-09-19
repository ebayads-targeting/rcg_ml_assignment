# Problem Statement

We have an e-commerce site. Users may browse the site with or without signing in with their userid. When a user makes a 
purchase, they are forced to login so that they can be authenticated for the purchase.

All the user activities on site are logged in a log file. The format of log file is a TAB separated text file with
following columns:

    EVENT_TIMESTAMP    COOKIE_ID    USER_ID    EVENT_TYPE    EVENT_DATA
    1505863678         cookie2      NULL       search        query=apple tv
    1505863778         cookie2      NULL       search        query=chromecast
    1505863679         cookie1      NULL       search        query=iphone x
    1505863733         cookie1      user1      purchase      item=iphoneX,count=1,amount=1200


All the actions that the user made without signing in will have only COOKIE_ID in the events logged in the log file.
All the actions that the user made after signing in will have both COOKIE_ID and USER_ID in the events logged in log file.

Design a program that will aggregate all events by user which will allow us to visualize the complete action path taken 
by a user to do a purchase. The output will be one JSON record per user. The JSON format is:

````
{
   "id": "user1",
   "events" : [ {
         "event_type": "search",
         "timestamp": 1505863679,
         "eventData": "query=iphone x"
     },
     {
         "event_type": "purchase",
         "timestamp": 1505863733,
         "eventData": "item=iphoneX,count=1,amount=1200"
     }
    ]
}
{
   "id": "cookie2",
   "events" : [ {
         "event_type": "search",
         "timestamp": 1505863678,
         "eventData": "query=apple tv"
     },
     {
         "event_type": "purchase",
         "timestamp": 1505863778,
         "eventData": "query=chromecast"
     }
    ]
}
````

Id is either USER_ID or COOKIE id. If all events are done by user without signing in, it is not possible to associate 
events to the user id. All such events will be grouped under COOKIE_ID. In above example, all events done by cookie2 are
without logging in. Hence all it's events are associated with id cookie2.

If user has done some events without signing in and some events after signing in, then events done without signing in 
can be grouped under USER_ID as all the events will have the COOKIE_ID same as events done after logging in. Example: user1.

NOTES:
- User can reset it’s COOKIE_ID any time by clearing Browser cookies
- Two user can share the computer, hence they can have the same COOKIE_ID
