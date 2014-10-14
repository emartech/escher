Implementation Guide
====================

Implementing the Escher protocol is not trivial. We have collected a strategy,
best practices and ideas we think will help you if you would like to create an
Escher implementation in a new programming language.

 * Create a group which can handle all requests
    * in a particular programming language and setting, http requests are available
      differently
    * it is useful if the SR has a representation independent of the information's framework request group
 * After you've created the parts of the header and request, you need to generate the canonical representation
 * Create the signature from the canonical request
 * If the signature is disposable, you can add it to a presigned URL to the request
