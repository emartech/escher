EscherJS
========

 * Source code: `EscherJS <https://github.com/emartech/escher-js>`_
 * npm package: `escher-auth <https://www.npmjs.org/package/escher-auth>`_

Installation
------------

We are supporting NPM as a package manager. If you have NodeJS installed
NPM will be very likely installed as well. Please check out the instructions
about `NPM installation <https://github.com/npm/npm#fancy-install-unix>`_,
if not.

If NPM is installed, you can install the *escher-auth* npm package by:

.. code-block:: bash

   npm install escher-auth

Usage
-----

To load the library, add the Composer autoloader to your code:

.. code-block:: javascript

   var Escher = require('escher-auth');

The library has 3 interfaces you can call. You can sign an HTTP request, you can presign a URL
and you can validate a signed HTTP request or presigned URL (with the same method).

Signing a request
^^^^^^^^^^^^^^^^^

Escher works by calculating a cryptographic signature of your request, and adding it (and other authentication
information) to the request.
Usually you will want to add the authentication information by appending extra headers to it.
Let's say you want to send a signed POST request to http://example.com/:

.. code-block:: javascript

   var escher = new Escher({
     credentialScope: 'example/credential/scope',
     accessKeyId: 'EscherExample',
     apiSecret: 'TheBeginningOfABeautifulFriendship'
   });

   var options = {
     host: 'example.com',
     port: 80,
     method: 'GET',
     url: '/validate_request',
     headers: [
       ['X-Escher-Date', (new Date).toUTCString()]
     ]
   }

   options = escher.signRequest(options, '');

   http.get(options, function(resp){
     resp.on('data', function(chunk){
       console.log(chunk.toString());
     });
   }).on("error", function(e){
     console.log("Got error: " + e.message);
   });

Presigning a URL
^^^^^^^^^^^^^^^^

.. code-block:: javascript

   var escher = new Escher({
     credentialScope: 'example/credential/scope',
     accessKeyId: 'EscherExample',
     apiSecret: 'TheBeginningOfABeautifulFriendship'
   });

   var presignedUrl = escher.preSignUrl('http://example.com/', 86400);

Validating a request
^^^^^^^^^^^^^^^^^^^^

You can validate a request signed by the methods described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts a function as a key database, where you can pass the client key, and it returns the client secret.

.. code-block:: javascript

   var escher = new Escher({
     credentialScope: 'example/credential/scope'
   });

   var keyDB = function(clientKey) {
     return "TheBeginningOfABeautifulFriendship";
   }

   escher.authenticate(request, keyDB);
