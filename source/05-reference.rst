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

Get users map
--------------

This call allows to get the number of users aggregated by area (postal code related).

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

Get app statistics
-------------------

This function allows to get the total points that have been awarded by the app.

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

This function allows to send a notification to one or more users of the app.

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

Requests coming from the frontend will have the possibility to make calls related to the user session. These kind of calls are made preferably from the frontend directly, but there might be some ocassions when doing it from the backend can be useful.

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

The functions that are available from the userSdk object are the same like in the frontend sdk. Please see Frontend SDK reference for details.


Frontend SDK
^^^^^^^^^^^^^

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