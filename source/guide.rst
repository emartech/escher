Guide
=====

An average size of an Escher implementation should be no more than 500-1000 lines of
code, depending on how granular methods you create. Every modern programming language
has the supporting libraries (like HMAC calculation) you will need, your job is to
go through the specification step-by-step and implement the methods.

We have tried our best to divide the tasks and create blocks you can implement in
some hours. This way you will be able to estimate the time you need for a full
implementation, and you also will have the idea the status of your code.

Also, there are language independent test cases you can run against your code. We
don't recommend starting with the tests, our experience says that's the harder way.
At the end, you should add the tests, and it will help a lot finalizing your
implementation.

Building blocks
---------------

This guide has to be finished, but already can give you some ideas. We are going to
add more details.

1. Implement a generic Request object
-------------------------------------

For a programming language, typically there's no general request representation. Some languages
has a common used request object, but for other languages every framework might have it's own
representation. It's a good practice creating an EscherRequest class you can instantiate with
different type of request objects, and it can provide an interface that Escher can use easily.

2. Create an Escher class
-------------------------

Create an Escher class you can instantiate with the following parameters, with their defaults.
Only the credential scope is required, others are optional:

  * credential_scope - no default
  * algo_prefix = 'ESR'
  * vendor_key = 'Escher'
  * hash_algo = 'SHA256'
  * current_time = Time.now
  * auth_header_name = 'X-Escher-Auth'
  * date_header_name = 'X-Escher-Date'
  * clock_skew = 900

3. Canonicalize a request
-------------------------

Create a method that can take a Request object, and create a canonicalized request, using
the Escher instances' parameters. Don't care about the details yet, but handle the basic cases
only. There will be a step later about implementing all the details of the specification. The
output should be something like at :ref:`canonicalized_request_example`.

4. Create a Signing String
--------------------------

Build a Signing String from the canonicalized request and the parameters of the Escher instance
object. Example output is available at :ref:`signing_string_example`. It should be very
straightforward to build the Signing String.

5. Create a Signing Key
-----------------------

Implement the Signing Key creation algorithm described at :ref:`signing_key_algorithm`. It
should be a short code. Note that the output will be a binary value.

6. Create the Signature
-----------------------

Create a signature from the Signing String and the Signing Key.

7. Add the Signature to the Headers
-----------------------------------

**TBD**

8. Canonicalize the path properly
--------------------------------

**TBD**

9. Canonicalize the query parameters properly
--------------------------------------------

**TBD**

10. Canonicalize the headers properly
-----------------------------------

**TBD**