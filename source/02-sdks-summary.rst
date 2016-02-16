SDKs Summary
=================

There are mainly two sdks that allow to integrate your app with KWS.

1. **Frontend SDK**: This SDK will help you interact with KWS from your frontend, allowing tasks like the following:
    * Forwarding unauthenticated users to the single sign-on web (a.k.a. "the Club").

    .. figure:: img/club.png

        (The Club is a single sign-on page specially designed for Kids safety and compliance)

    * Make requests to KWS like getting the user's profile and points, triggering events that award points to users, getting leaderboards, etc.

    * Show notifications to the user.

2. **Node.js SDK**: This SDK will help you to integrate easily your Node.js application with KWS. It will handle user authentication integration with KWS and it will also provide some additional functions to interact with KWS to get data like your app statistics.