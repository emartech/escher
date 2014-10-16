Configuring Escher
==================

You have to have some service specific constants when you would like to use
Escher. If you are implementing a client, it will be provided by the service
owner. If you are a service owner, you have to define them for your users.

.. note::

   In practice, you should only define the credential scope, and keep the
   defaults of the other options.

credential scope
^^^^^^^^^^^^^^^^

The most important service constant is the *credential scope*, this is the
only required parameter when you are using the library. It is a
slash separated, hierarchical id, containing the service's scope. Amazon
uses it to identify the data center, the service and the protocol. An
Amazon example is "us-east-1/iam/aws4_request". You should define a scope
like "eu-vienna/yourproductname/escher_request".

clock_skew
^^^^^^^^^^

This option defines the maximum allowed window of the difference between
the clock of the client and the server, as it's quite typical that
computer times are not synced well. Without this setting, even small
differences could cause the invalidity of the signature.

The default is ±900 secs (15 mins), it generally works well. You might
want to allow smaller difference only.

algo_prefix, vendor_key, auth_header_name and date_header_name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These options are implemented to keep the compability with the Amazon AWS4
protocol, it is not advised to change them.

hash_algo
^^^^^^^^^

Defines the hash algorithm used to calculate the signature. There are two
supported values: *SHA256* and *SHA512*.

current_time
^^^^^^^^^^^^

This parameter is only for testing purposes, as tests need to have an injected
time for repeatability reasons.