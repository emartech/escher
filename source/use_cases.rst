Use Cases
=========

We have collected some use cases where we think Escher is a great fit.

REST API Authentication
-----------------------

There are popular solutions for API authentication, but we think Escher is the best fit, because

 * it's stateless,
 * the communication not includes the password,
 * for a middle man it's not possible to modify the request,
 * *and* it's REST friendly by including the HTTP method in the checksum calculation.

It's stateless
^^^^^^^^^^^^^^

Being stateless means that the server does the authentication in a way that it doesn't have to maintain
a dynamic database with API sessions.

No password or token included
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Using other authentication methods like HTTP Basic Authentication or OAuth, access to the
communication will reveal the password or a token valid for a longer time. The Escher
protocol does not includes any secret, just a checksum based on your secret. This gives you
a higher level of security.

No modifications
^^^^^^^^^^^^^^^^

As Escher includes all the important parts of the request when it calculates the checksum,
modifying the request will invalidate the signature. It means
that a middle man might be able to read the communication, but cannot modify it.

REST friendly
^^^^^^^^^^^^^

Sending requests with the proper HTTP method and URIs is integral part of REST API calls.
Because Escher checksum calculation includes the HTTP method and the other typical REST HTTP
parameters, it won't be possible to send a REST API request differently.


Protected resources
-------------------

If you have to give temporary access within a timeframe for a resource to your users,
Escher can help you with presigned URLs.


Single-sign-on, Integrations
----------------------------

While it's not Escher's goal to provide a single-sign-on solution, with presigned URLs
you can create links between services that allow the target service to login the user
properly. It works well when you open a new window for the other service, and also
when you would like to display/include the service in an iframe inside your service.




