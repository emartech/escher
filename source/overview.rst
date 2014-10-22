Overview
========

This page just a short overview about this process, please read the
:doc:`specification' for details.

Signing a request
-----------------

Signing an HTTP request is only a few steps.

1. Canonicalizing a request
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Maybe this is the most difficult step, creating the canonicalized version
of the HTTP requests according to the specification needs a lot of code.

Let's start with this HTTP request:

.. code-block:: http

   POST /path/resource/?foo=bar&abc=efg HTTP/1.1
   Accept: */*
   User-Agent: example-client
   Connection: close
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 21
   Host: example.com

   message=Hello%20World

Escher creates a string from it, that represents the request in a canonicalized form. The
string is based on the different parts of the request, separated by line breaks (``\n``).
Each parts are canonicalized, for example the header names are lower cased and ordered,
the query parameters are ordered, and so.

Our example request's will look something like this after canonicalization:

.. code-block:: string

   POST
   /path/resource/
   foo=bar&abc=efg
   accept:*/*
   user-agent:example-client
   connection:close
   content-type:application/x-www-form-urlencoded
   content-length:21
   host:example.com
   date;host
   fedcba9876543210fedcba9876543210fedcba9876543210fedcba9876543210

2. Calculating the signature
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The next step includes creating a HMAC checksum from the canonicalized request string,
and creating an other new line separated string, now including the algorithm id,
the current time, the credential scope and the canonicalized request's checksum.

It will look something like this:

.. code-block:: string

   ESR-HMAC-SHA256
   20141022T120000Z
   20141022/eu-vienna/yourproductname/escher_request
   0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef

Also we are creating a key to calculate the signature. It is based on the API secret,
the algo_prefix ("ESR" by default), the current date, and the credential scope.

Escher takes the date and the parts of credential scope, and calculates a checksum
with each part, on the algo_prefix and API secret:

.. code-block:: ruby

   signing_key = algo_prefix + API_secret
   key_parts = [current_date] + credential_scope.split('/')
   key_parts.each { |data|
     signing_key = Digest::HMAC.digest(data, signing_key, algo)
   }
   return signing_key

At the end, with the string above, and this signing_key, it calculates a checksum
with HMAC.

3. Adding the signature to the HTTP headers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The final step is adding the signature to the request, as a new header. If the request
has no host, or has no date header, they have to be added, too.

At the end of the process, the HTTP request will be like this:

.. code-block:: http

   POST /path/resource/?foo=bar&abc=efg HTTP/1.1
   Accept: */*
   User-Agent: example-client
   Connection: close
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 21
   Host: example.com
   X-Escher-Date: 20141022T120000Z
   X-Escher-Auth: ESR-HMAC-SHA256 Credential=API_KEY/20141022/eu-vienna/yourproductname/escher_request,
     SignedHeaders=host;x-esr-date,
     Signature=abcdef01234567890abcdef01234567890abcdef01234567890abcdef0123456

   message=Hello%20World

More
----

To get more information, you can read our detailed :doc:`specification`, our check out one of the
:doc:`implementations`.
