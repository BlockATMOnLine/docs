# Access Instructions

Welcome to BlockATM API Documentation! BlockATM offers a set of powerful and flexible APIs that allow developers to integrate seamless cryptocurrency payment services into their applications.

Most of our endpoints will require an API key as a way to authenticate the call. BlockATM provides 3 different types of API&#x20;



* Public for non sensitive endpoints, those can be made by your frontend clients. Authentication is performed via a query string parameter.
* Server-to-Server for endpoints that would ideally be called directly from your backend servers. For the Query api, you only need to provide the apikey in request header.
* For creat order Api, you not only need to provide the apikey, but you also need to sign the request data.
