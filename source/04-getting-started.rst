Getting Started
================

Node.js SDK Integration
^^^^^^^^^^^^^^^^^^^^^^^^

The Node.js SDK integrates very easily with apps based on express.js

**Step 1.** Install the latest version of KWS Node.js SDK in your project

.. code-block:: shell

    npm install --save "git+https://github.com/SuperAwesomeLTD/sa-kws-node-sdk.git#8ea47cf291cc54a8413f1321262f450f4feb7cb7"

**Step 2.** Create an instance of the SDK in your code

.. code-block:: javascript

    var KwsSdk = require('sa-kws-node-sdk');

    // KWS will provide all the necessary info for your account
    var kwsSdk = new KwsSdk({
        clientId: 'your_client_id',
        apiKey: 'your_api_key',
        kwsApiHost: 'https://examplekwsapihost.com'
    });

**Step 3.** Include the callback handlers for user authentication

.. code-block:: javascript

    // assuming that app is your express instance object
    app.use(kwsSdk.router);

Frontend SDK Integration
^^^^^^^^^^^^^^^^^^^^^^^^^

**Step 1.** Include SDK and dependencies in your html file

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

**Step 2.** Create an instance of the SDK in your script

.. code-block:: html

    <script type="text/javascript">

        // KWS will provide all the necessary info for your account
        var kwsSdk = new KwsSdk({
            clientId: 'your_client_id',
            clubUrl: 'https://examplekwsclub.com',
            kwsApiHost: 'https://examplekwsapihost.com',
            language: 'en' // The language of your frontend here
        });

        // Now you can use the SDK to make API calls. See reference section
        kwsSdk.user.get()
            .then(function (userData) {
                console.log('Got user data', userData);
            })
            .fail(function () {
                console.log('Call failed. User is not authenticated');
            })
            .always(function () {
                console.log('This is always called at the end when the response is received');
            });

    </script>
