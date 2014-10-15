Guide
=====

An average size of an Escher implementation should be about 500-1000 lines of code,
depending on how granular methods you create. Every modern programming language has
the supporting libraries (like HMAC calculation) you will need, your job is to
go through the specification step-by-step and implement the methods.

We have tried our best to divide the tasks and create blocks you can implement in
some hours. This way you will be able to estimate the time you need for a full
implementation, and you also will have the idea the status of your code.

Building blocks
---------------

Just follow our guide:

1. Implement a generic Request object
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Typically there's no general request representation. For some programming language there's
a common used request object, but for other languages every framework might have it's own
representation. It's a good practice creating an EscherRequest class you can instantiate with
request objects, and it can provide an interface that Escher can use easily.

 * Create a group which can handle all requests
    * in a particular programming language and setting, http requests are available
      differently
    * it is useful if the SR has a representation independent of the information's framework request group
 * After you've created the parts of the header and request, you need to generate the canonical representation
 * Create the signature from the canonical request
 * If the signature is disposable, you can add it to a presigned URL to the request
