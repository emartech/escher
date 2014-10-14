Using Escher
============

You have to have some service specific constants when you would like to use
Escher. If you are implementing a client, it will be provided by the service
owner. If you are a service owner, you have to define them for your users.

The most important service constant is the **credential scope**. It is a
slash separated, hierarchical id, containing the service's scope. Amazon
using it to identify the data center, the service and the protocol. An
Amazon example is "us-east-1/iam/aws4_request", while you can define a scope
like "eu-vienna/*yourproductname*/escher_request".

.. note::
   While all the implementation should allow you to modify the algo prefix,
   vendor key and authorization, date header names, you should keep the
   defaults.

Setting Options
===============

 * algo_prefix
Algorithm prefix which defines the algorithm. It is not advised to be changed.
Can be changed in order to use this library with the services of amazon.
 * vendor
Belongs to the algorithm, it can be changed only because of the amazon compatibility.
 * hash_algo
Defines the algorithm of the signature. 2 values are supported: SHA256 and SHA252
 * current_time
testing purposes
 * auth_header_name
 * date_header_name
Defines how to transfer authentication and date information in the http headers
 * clock_skew
Handles the difference between the clock of the client and the server. Small differences can cause the invalidity of the signature.
A timeframe can be configured, so the server accepts the request in the case of time difference as well.