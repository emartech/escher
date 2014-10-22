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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For a programming language, typically there's no general request representation. Some languages
has a common used request object, but for other languages every framework might have it's own
representation. It's a good practice creating an EscherRequest class you can instantiate with
different type of request objects, and it can provide an interface that Escher can use easily.

2. Canonicalize a request, part 1
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create a method that can take a Request object, and create a canonicalized request. Don't care
about the details yet, but handle the basic cases only. There will be a step later about
implementing all the details of the specification. The output should be something like
at step *2.1.7.* of the :ref:`canonicalized_request_example`.
