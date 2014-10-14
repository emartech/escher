Checklist for Auditing
======================

Are you ready with your implementation, and would like to double check if it works
well and secure? First, you should run the test package, but we are also providing
you this checklist you can go through and audit your code.

Request Signing
---------------

**Request canonization**

It has to canonicalize the request properly.

 * Is it correctly parsing *request URI* into *path* and *query*?
 * Is it upper casing the HTTP method?
 * Is it normalizing paths correctly?
 * Is it normalizing query string correctly?
    * Should URL encode query keys
    * Should URL encode query values
    * Should handle empty value correctly
    * Should sort keys
 * Is it normalizing headers correctly?
    * Should lowercase header names
    * Should trim value beginning
    * Should trim value ending
    * Should keep spaces when they are inside quotes



