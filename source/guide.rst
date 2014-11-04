Guide
=====

An average size of an Escher implementation should be no more than 500-1000 lines of
code, depending on how granular are the methods you create. Every modern programming language
has the supporting libraries (like HMAC calculation) you will need, your job is to
go through the specification step-by-step and implement the methods.

We have tried our best to divide the tasks and create blocks you can implement within
some hours. This way you will be able to estimate the time you need for a full
implementation, and you also will have the status of your code.

Also, there are language independent test cases you can run against your code. We
don't recommend starting with the tests. According to our experience, that's the harder way.
At the end, you should add the tests, which will help a lot finalizing your
implementation.

Building Blocks
---------------

This guide has to be finished, but already can give you some ideas. We are going to
add more details. You can always check one of the implementations to get more ideas
about how it works. At the moment, the EscherRuby implementation is in the best
shape.

1. Implement a Generic EscherRequest Class
------------------------------------------

For a programming language, typically there's no general request representation. Some languages
have a commonly used request object, but for other languages, every framework might have its own
representation. It is also typical that the request object a client creates is different from
the request object a controller on the server side receives.

It's a good practice creating an EscherRequest class you can instantiate with different types of
request objects. It can provide an interface that Escher can use easily.

The Request class should be instantiated with request objects, and provide the following interface
for the Escher implementation:

 * get the HTTP method (a string)
 * get the Headers (should be an array of key-value pairs)
 * get the Host (a string)
 * get the Path (a string starting with /, not including the question mark, query parameters)
 * get the Query parameters (should be an array of key-value pairs)
 * get the Body (a string)
 * set a Header
 * has a Header?
 * give back the request (with possible modifications via header settings, but as the original object's class)

2. Create an Escher Class
-------------------------

Create an Escher class you can instantiate with the following parameters and their defaults.
Only the credential scope is required, the others are optional:

  * credential_scope - no default
  * algo_prefix = 'ESR'
  * vendor_key = 'Escher'
  * hash_algo = 'SHA256'
  * current_time = Time.now
  * auth_header_name = 'X-Escher-Auth'
  * date_header_name = 'X-Escher-Date'
  * clock_skew = 900

3. Canonicalize a Request
-------------------------

Create a method that can take a request object, and create a canonicalized request, using
the Escher instances' parameters. Don't care about the details yet, but handle the basic cases
only. There will be a step later about implementing all the details of the specification. The
output should be something like at :ref:`canonicalized_request_example`.

4. Create a Signing String
--------------------------

Build a signing string from the canonicalized request and the parameters of the Escher instance
object. Example output is available at :ref:`signing_string_example`. It should be very
straightforward to build the signing string.

5. Create a Signing Key
-----------------------

Implement the signing key creation algorithm described at :ref:`signing_key_algorithm`. It
should be a short code. Note that the output will be a binary value.

6. Create the Signature
-----------------------

Create a signature from the signing string and the signing key.

7. Add the Signature to the Headers
-----------------------------------

Call the EscherRequest's header settings, and set a proper Auth header, according to
:ref:`add the signature`.

8. Canonicalize the Path Properly
---------------------------------

Now go back to your path canonicalization, and fine tune it according to the specification.
Most of the time, ready-to-use libraries will be available for you, but sometimes they are
not fully compatible with the standards or the Escher specification.

9. Canonicalize the Query Parameters Properly
---------------------------------------------

And fine tune the query parameter canonicalization as well. If EscherRequest provides
the query parameters as an array, then

10. Canonicalize the Headers Properly
-------------------------------------

**TBD**