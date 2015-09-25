Configuring Escher
==================

You have to have some service specific constants when you would like to use
Escher. If you are implementing a client, it will be provided by the service
owner. If you are a service owner, you have to define them for your users.

.. note::

   In practice, you should only define the credential scope, and keep the
   defaults of the other options.

Credential Scope
^^^^^^^^^^^^^^^^

The most important service constant is the *credential scope*, this is the
only required parameter when you are using the library. It is a
slash separated, hierarchical ID, containing the service's scope. Amazon
uses it to identify the data center, the service and the protocol. An
Amazon example is ``us-east-1/iam/aws4_request``. You should define a scope
like ``eu-vienna/yourproductname/escher_request``.

Clock Skew
^^^^^^^^^^

This option defines the maximum allowed window of the difference between
the clock of the client and the server, as it's quite typical that
computer times are not synced well. Without this setting, even small
differences could cause the invalidity of the signature.

The default is ±900 secs (15 mins), it generally works well. You might
want to allow smaller difference only.

Algo Prefix, Vendor Key, Authentication Header Name and Date Header Name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These options are implemented to keep the compability with the Amazon AWS4
protocol, it is not recommended to change them.

Hash Algo
^^^^^^^^^

Defines the hash algorithm used to calculate the signature. There are two
supported values: *SHA256* and *SHA512*.

Current Time
^^^^^^^^^^^^

This parameter is only for testing purposes, as tests need to have an injected
time for repeatability reasons.

Possible error messages
^^^^^^^^^^^^^^^^^^^^^^^

Message:
  The date header is missing

Solution:
  The "dateHeaderName" configuration should be the same on server and client.
  
----

Message:
  The authorization header is missing

Solution:
  The "authHeaderName" configuration should be the same on server and client.
  
----

Message:
  The host header is missing

Solution:
  The client's Escher implementation is incomplete or the host header lost between the server and the client.
  
----

Message:
  Could not parse auth header

Solution:
  The client's Escher implementation is incomplete. `Specification for authorization header <http://escherauth.io/specification.html#adding-the-signature-to-the-request>`_.
  
----

Message:
  The host header is not signed

Solution:
  The client's Escher implementation is incomplete.
  
----

Message:
  The date header is not signed

Solution:
  The client's Escher implementation is incomplete.
  
----

Message:
  The credential scope is invalid

Solution:
  The "credentialScope" configuration should be the same on server and client.
  
----

Message:
  Only SHA256 and SHA512 hash algorithms are allowed

Solution:
  The client's Escher implementation is incomplete. Escher only supports these hash algorithms.
  
----

Message:
  The authorization header's shortDate does not match with the request date

Solution:
  The client's Escher implementation is incomplete. The authorization header's short date is not equal with the request date header.
  
----

Message:
  The request date is not within the accepted time range

Solution:
  The server's or client's time is out of sync. Use an NTP (Network Time Protocol) client or a similar solution to set the accurate time.
  
----

Message:
  Invalid Escher key

Solution:
  The client is using an Escher key missing on the server.
  
----

Message:
  The signatures do not match

Solution:
  The most likely problem is either the Escher secret is wrong or the signed and sent requests are different.
