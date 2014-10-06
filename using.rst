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
