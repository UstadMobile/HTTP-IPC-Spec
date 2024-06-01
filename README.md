# Offline REST Recipe for Native Apps

[Diagram](img/offline-rest-flow-a.svg)

Scenario: 

The user has an API provider app (e.g. a Learning Management System app) and a client app (e.g. a math app). In the online world there are well defined REST HTTP APIs that would enable these apps to talk to each other; typically using [OAuth](https://oauth.net/2/) for the client to get a token, and then using a REST API to access information (e.g. student grade level and class enrollment information using [OneRoster](https://www.1edtech.org/standards/oneroster)).

[RFC8252](https://datatracker.ietf.org/doc/html/rfc8252) describes how native apps can use OAuth to get an access token.

This recipe works as follows:

* The client app launches the link e.g. ```https://auth-server.com/auth``` which will launch the authorization app if it is installed (using App Links on Android and claimed https URIs on iOS).

* The authorization app allows the user to accept or deny the request. If the user accepts it launches the redirect link specified by the client e.g. ```com.example.app://auth-done``` and includes a ```code``` parameter to redeem for a token (as per standard OAuth) and an ```ipc-service``` parameter.

* The client app binds to the service specified by the ```ipc-service``` parameter (using a [bound service](https://developer.android.com/develop/background-work/services/bound-services) on Android or [XPC service](https://developer.apple.com/documentation/xpc) on iOS).

* The client app sends a REST API request to the API app (which may, as per the OAuth spec, be the same as the authorization app) by serializing the HTTP request into bytes which are then received by the API app, which in turn serializes its response into bytes for the client app. The client app can now use the same API it would use in an online scenario without requiring Internet access.

* The client app MAY use a library such that it can access the API using normal http via an embedded localhost server instead of directly serializing http requests itself.

Status:

This is a very rough draft intended to facilitate discussion and comments. Issues, suggestions etc. are welcome

