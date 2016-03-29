Reference
==========

Node.js SDK
^^^^^^^^^^^^^

Instantiating the SDK
----------------------

.. code-block:: javascript

    var KwsSdk = require('sa-kws-node-sdk');

    // KWS will provide all the necessary info for your account
    var kwsSdk = new KwsSdk({
        clientId: 'your_client_id',
        apiKey: 'your_api_key',
        kwsApiHost: 'https://examplekwsapihost.com'
    });

Adding controller handlers for oauth callbacks
-----------------------------------------------

.. code-block:: javascript

    // assuming that app is your express instance object
    app.use(kwsSdk.router);

Get Users Map
--------------

This call allows you to get the number of users aggregated by area (postal code related).

* Function: **app.user.getMap**

* Parameters:

    * **countryCode**: (string, required) ISO 3166-1 2 chars code

* Response: It depends on the country. This could be an example for UK

.. code-block:: json

    {
        "n1": 200,
        "n12": 200,
        "n233": 200,
        "n244": 200,
        "n355": 200,
        "n366": 55,
        ...
    }


* Example:

.. code-block:: javascript

    kwsSdk.app.user.getMap({countryCode:'GB'})
        .then(function (data) {
            // Your resp handler here
        })
        .catch(function (err) {
            // Your error handler here
        });

Get App Statistics
-------------------

This function allows you to get the total points that have been awarded by the app.

* Function: **app.getStatistics**

* Parameters: none

* Response:

    * **totalPointsAwarded** (Number)

    Example:
.. code-block:: json

    {
        "totalPointsAwarded": 1054782
    }

* Example:

.. code-block:: javascript

    kwsSdk.app.getStatistics()
        .then(function (data) {
            // Your resp handler here
        })
        .catch(function (err) {
            // Your error handler here
        });

Notify
-------

This function allows you to send a notification to one or more users of the app.

* Function: **app.notify**

* Parameters:

    * **targetUserIds** (Array of integers, required) ids of the target users
    * **description** (string, required) message to be sent (html accepted)

* Response: Empty

* Example:

.. code-block:: javascript

    kwsSdk.app.notify({
        targetUserIds: [125],
        description: 'Thank you!<br/><br/>Your welcome package has been sent!'
    }).then(function () {
        // Your resp handler here
    }).catch(function (err) {
        // Your error handler here
    });

User functions
---------------

Requests coming from the frontend will have the possibility to make calls related to the user session. These kind of calls are preferably made from the frontend directly, but there might be some ocassions when doing it from the backend can be useful.

The following example is a middleware that injects the user profile data in the req object:

.. code-block:: javascript

    app.use(function (req, res, next) {

        // saUserSdk is now injected in the req obj and allows to make calls with the user session
        req.saUserSdk.user.get()
            .then(function (userData) {
                req.user = userData;
                next();
            })
            .catch(function () {
                // No user data could be fetched
                req.user = null;
                next();
            });
    });

The functions that are available from the userSdk object are the same as in the frontend SDK. Please see Frontend SDK reference for details.


Frontend SDK
^^^^^^^^^^^^^

Dependencies
-------------

THe frontend sdk has the following dependencies

* jQuery 1.8+ (necessary for ajax calls)
* font-awesome 4.4.0+ (necessary to display notifications)

Including the sources in your html
-----------------------------------

.. code-block:: html

    <head>
        <!-- (...) -->

        <!-- KWS SDK STYLES AND DEPENDENCIES-->
        <link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css" rel="stylesheet" type="text/css">
        <link href="https://s3-eu-west-1.amazonaws.com/sa-kws-frontend-sdk/v1.0.0/sa-kws-frontend-sdk-1.0.0.min.css" rel="stylesheet" type="text/css">
    </head>

    <body>
        <!-- (...) -->

        <!-- KWS SDK AND DEPENDENCIES-->
        <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.0/jquery.min.js"></script>
        <script type="text/javascript" src="https://s3-eu-west-1.amazonaws.com/sa-kws-frontend-sdk/v1.0.0/sa-kws-frontend-sdk-1.0.0.min.js">
    </body>


Instantiating the SDK
----------------------

.. code-block:: javascript

    var KwsSdk = require('sa-kws-node-sdk');

    // KWS will provide all the necessary info for your account
    var kwsSdk = new KwsSdk({
        clientId: 'your_client_id',
        apiKey: 'your_api_key',
        kwsApiHost: 'https://examplekwsapihost.com',
        language: 'en'  // The language of your frontend here
    });

Get User Profile and Points
----------------------------

This function allows you to get the user's profile and their points

* Function: **user.get**

* Parameters: none

* Response:

    * **id** (Number): id of the user.
    * **username** (string): specific display name for the app (it is app specific for every user).
    * **applicationPermissions** (Object): description of permissions to access data. Each attribute will be a boolean indicating permission to access every specific piece of data of the child.
    * **applicationProfile** (Object): profile specific to the app. Includes the following:
        * **username** (string): specific display name for the app (it is app specific for every user).
        * **avatarId** (Number): avatar identifier for the user (apps have to handle it).
        * **customField{x}** (Number): number to be used by the app to store/get non-personal info for the user like badges for example.
    * **dateOfBirth** (string YYYY-MM-DD).
    * **language** (string) ISO 639-1 code.
    * **gender** (string) 'm' for male and 'f' for female.
    * **points** (Object) object with a summary of the points of the user. Inlcudes the following fields:
        * **availableBalance** (Number) The current number of points available for the user.
        * **pending** (Number) The amount of points that are blocked for the user.
        * **total** (Number) The total current number of points for the user (including the blocked ones).
        * **totalPointsReceivedInCurrentApp** The total number of points that the user received in this app.
        * **totalReceived** The total number of points that the user received across all apps.
    * **firstName** (string, optional) This field will only be available if the corresponding permission is allowed.
    * **lastName** (string, optional) This field will only be available if the corresponding permission is allowed.
    * **phoneNumber** (string, optional) This field will only be available if the corresponding permission is allowed.
    * **email** (string, optional) This field will only be available if the corresponding permission is allowed.
    * **address** (Object, optional) This field will only be available if the corresponding permission is allowed. It would contain the following fields:
        * **street** (string)
        * **postCode** (string)
        * **city** (string)
        * **country** (string)

* Response example:

.. code-block:: json

    {
        "id": 2,
        "applicationPermissions":{
            "accessAddress":true,
            "accessFirstName":true,
            "accessPhoneNumber":false,
            "accessEmail":false,
            "accessLastName":false
        },
        "applicationProfile": {
            "username": "user2",
            "avatarId": 0,
            "customField1": 0,
            "customField2": 0,
            "customField3": 0,
            "customField4": 0,
            "customField5": 0
        },
        "username": "user2",
        "dateOfBirth":"2005-03-07",
        "language":"en",
        "gender":null,
        "firstName":"",
        "address": {
            "street":"Example Street",
            "postCode":"11111",
            "city":"Example City",
            "country": "GB"
        },
        "points": {
            "availableBalance": 515,
            "pending": 0,
            "total": 515,
            "totalPointsReceivedInCurrentApp": 119,
            "totalReceived": 515
        }
    }

* Example:

.. code-block:: javascript

    kwsSdk.user.get()
        .then(function (userData) {
            // Your resp handler here
        })
        .fail(function (err) {
            // Your error handler here
        });

Update User Profile
--------------------

This function allows apps to update the application profile of the user (excluding the display name, which is inmutable)

* Function: **user.update**

* Parameters:

    * **applicationProfile** (Object): profile specific to the app. Includes the following:
        * **avatarId** (Number, optional): avatar identifier for the user (apps have to handle it).
        * **customField{x}** (Number, optional): number to be used by the app to store/get non-personal info for the user like badges for example.

* Response: empty

* Example:

.. code-block:: javascript

    kwsSdk.user.update({
        applicationProfile: {
            avatarId: 1,
            customField1: 3,
            customField2: 2,
            customField3: 1
        }
    }).then(function () {
        // Your resp handler here
    }).fail(function (err) {
        // Your error handler here
    });

Trigger event
--------------

This function allows your app to award points to users when a certain event happens (like watching a video or playing a game)

* Function: **user.triggerEvent**

* Parameters:

    * **token** (string): token string that identifies the event (KWS will provide you with this)
    * **points** (number, optional) if omitted, the deafult amount of points will be given
    * **description** (string, optional): Message the will be sent to the user in a notification. It can contain HTML (This message can contain the word {{POINTS}} that will be replaced by the corresponding amount of points).

* Response: empty

* Example:

.. code-block:: javascript

    kwsSdk.user.triggerEvent({
        token: "aaabbbcccddd",
        points: 3,
        decription: "Thanks for watching! <br> You have been awarded {{POINTS}}!"
    }).then(function () {
        // Your resp handler here
    }).fail(function (err) {
        // Your error handler here
        // Errors will be usual in this call, because there are limits when awarding points to users with the same token
    });

Check if event has been triggered
----------------------------------

This function is used to check if an event has already reached the limit for the user, and thus will not award points anymore.

* Function: **user.hasTriggeredEvent**

* Parameters:

    * **eventId** (number): event identifier

* Response:

    * **hasTriggeredEvent** (boolean) It will indicate if the event has reached the limit or not.

* Example:

.. code-block:: javascript

    kwsSdk.user.hasTriggeredEvent({
        eventId: 25
    }).then(function (resp) {
        // Your resp handler here
    }).fail(function (err) {
        // Your error handler here
    });

Request Permissions
--------------------

This function allows your app to send a notification to a user's parent to request permission for a specific feature, like sending a welcome pack or a birthday email.

* Function: **user.requestPermissions**

* Parameters:

    * **permissions** (array of strings): array with the requested permissions. It can include the following strings:
        * accessEmail
        * accessAddress
        * accessFirstName
        * accessLastName
        * accessPhoneNumber

* Response: empty

* Example:

.. code-block:: javascript

    kwsSdk.user.requestPermissions({
        permissions: ['accessAddress']
    }).then(function (resp) {
        // Your resp handler here
    }).fail(function (err) {
        // Your error handler here
    });

Invite a Friend
----------------

This function allows a user to invite a friend to the app by providing their email. The user making the invite will get rewarded with points when the new user joins the app.

* Function: **user.inviteUser**

* Parameters:

    * **email** (strings): email of the user's friend.

* Response: empty

* Example:

.. code-block:: javascript

    kwsSdk.user.inviteUser({
        email: "myfriend@example.com"
    }).then(function (resp) {
        // Your resp handler here
    }).fail(function (err) {
        // Your error handler here
    });

Get Score
----------

This function allows you to get a user's score and rank in the app. The score can be filterd by date

* Function: **app.getScore**

* Parameters:

    * **start** (number, optional): lower threshold timestamp (ms) for the date filter
    * **end** (number, optional) upper threshold timestamp (ms) for the date filter

* Response:

    * **points** (number) number of points of the user in the app
    * **rank** (number) rank of ther user in the app

* Example response:

.. code-block:: json

    {
        "rank": 11,
        "score": 2675
    }

* Example:

.. code-block:: javascript

    kwsSdk.app.getScore()
        .then(function (resp) {
            // Your resp handler here
        })
        .fail(function (err) {
            // Your error handler here
        });

Get Leaderboard
----------------

This function allows you to get a leaderboard for the app. It can be filtered by date, including day, week and month leaderboards for example. The user does not have to be authenticated in order to make this call.

* Function: **app.leader.list**

* Parameters:

    * **offset** (number, optional): offset of the leaderboard results (for the paged results. 0 by default)
    * **limit** (number, optional): limit of the leaderboard results (for the paged results. 0 by default)
    * **start** (number, optional): lower threshold timestamp (ms) for the date filter
    * **end** (number, optional) upper threshold timestamp (ms) for the date filter

* Response:

    * **offset** (number) offset of the results
    * **limit** (number) limit length applied to the results
    * **count** (number) number of total users (if limit were not applied)
    * **results** (array) paged results. Every entry is an object with the following attributes:
        * **rank** (numnber) rank of the user

* Example response:

.. code-block:: json

    {
        "offset": 0,
        "limit": 50,
        "count": 2,
        "results": [
            {
                "rank": 1,
                "score": 4386,
                "user": "user321"
            },
            {
                "rank": 2,
                "score": 1235,
                "user": "user132"
            }
        ]
    }

* Example:

.. code-block:: javascript

    kwsSdk.app.leader.list({
        start: 1454284800000,
        end: 1456790400000
    }).then(function (resp) {
        // Your resp handler here
    })
    .fail(function (err) {
        // Your error handler here
    });

Get App Data
----------------

This function allows you to retrieve previous stored data related to the user in that specific app.
The data is in the form of pair key-values. It can be filtered by key name.

* Function: **app.user.appData.list**

* Parameters:

    * **offset** (number, optional): offset of the data results (for the paged results. 0 by default)
    * **limit** (number, optional): limit of the data results (for the paged results. 0 by default)
    * **name** (string, optional): search a value by that specific key name

* Response:

    * **offset** (number) offset of the results
    * **limit** (number) limit length applied to the results
    * **count** (number) number of total variables (if limit were not applied)
    * **results** (array) paged results. Every entry is an object with the following attributes:
        * **name** (string) variable name
        * **value** (number) variable value


* Example response:

.. code-block:: json

    {
        "offset": 0,
        "limit": 50,
        "count": 1,
        "results": [
            {
                "name": "timeTillWorldEnds",
                "value": 2864212
            }
        ]
    }

* Example:

.. code-block:: javascript

    kwsSdk.app.user.appData.list({
        name: "timeTillWorldEnds"
    }).then(function (resp) {
        // Your resp handler here
    })
    .fail(function (err) {
        // Your error handler here
    });


Set App Data
----------------

This function allows you to create or update data related to the user in that specific app.
The data is in the form of pair key-values. Values can only be integers.

* Function: **app.user.appData.set**

* Parameters:

    * **name** (string, optional): key name
    * **value** (integer, optional): value to be stored

* Response: empty

* Example:

.. code-block:: javascript

    kwsSdk.app.user.appData.set({
        name: "timeTillWorldEnds",
        value: 2864212
    }).then(function (resp) {
        // Your resp handler here
    })
    .fail(function (err) {
        // Your error handler here
    });

Delete App Data
----------------

This function allows you to delete data related to the user in that specific app.

* Function: **app.user.appData.deleteByName**

* Parameters:

    * **name** (string, optional): key name of data you want to delete

* Response: empty

* Example:

.. code-block:: javascript

    kwsSdk.app.user.appData.deleteByName({
        name: "timeTillWorldEnds"
    }).then(function (resp) {
        // Your resp handler here
    })
    .fail(function (err) {
        // Your error handler here
    });
