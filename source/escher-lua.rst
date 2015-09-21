EscherLua
========

 * Source code: `EscherLua <https://github.com/emartech/escher-lua>`_

Usage
-----

To load the library, add the Composer autoloader to your code:

.. code-block:: lua

   local Escher = require("lib.escher")

The library has one interface you can call. You can validate a signed HTTP request.

Authenticate the HTTP request
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The authenticate function examine the HTTP request and return with an error in the following cases:

 * There is a missing header in the request.
 * Algorithm prefix is invalid.
 * Algorithm is not supported.
 * Invalid API key.
 * The request date and the date in the request's credential do not match.
 * The request date is not within the accepted time range.
 * Invalid credentials.
 * Host header is not signed.
 * Date header is not signed.
 * The signatures do not match.

If there is no error in the request the authentication process returns with the given client key.

Validating a request
^^^^^^^^^^^^^^^^^^^^

You can validate a request signed by the method described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts a function as a key database, where you can pass the client key, and it returns the client secret.

.. code-block:: lua

   local escher = Escher:new(getConfigFromTestsuite(test.config))
   
   local getApiSecret = function(key)
          for _, element in pairs(test.keyDb) do
            if element[1] == key then
              return element[2]
            end
          end
        end
        
   local apiKey, err = escher:authenticate(test.request, getApiSecret)

Possible error codes
^^^^^^

.. list-table:: 
   :header-rows: 1

   * - Message
     - Description
     - Fix
   * - The date header is missing.
     - The required date header is missing.
     - Add the right date header to the request.
   * - The auth header is missing.
     - The required authorization header is missing.
     - Add the right authorization header to the request.
   * - The host header is missing.
     - The required host header is missing.
     - Add host header to the request.
   * - Could not parse auth header.
     - There is a problem with authorization header, the parseAuthHeader can't parse it.
     - Check the authorization header. It should separate by ',' and should contains: algorithm, credential, signedheaders, signature informations.
   * - The host header is not signed.
     - Host is not part of the authorization header's signedHeaders parameter.
     - Check the authorization header's signedHeaders parameter, host must be there.
   * - The date header is not signed.
     - Date is not part of the authorization header's signedHeaders parameter.
     - Check the authorization header's signedHeaders parameter, date must be there.
   * - The credential scope is invalid.
     - Credential scope has not enough or some misspelled element.
     - Check the credential in authorization header's Credential parameter (Credential='key'/'date'/'credential',).
   * - Only SHA256 and SHA512 hash algorithms are allowed.
     - The used algorithm for the presigned signature is not accepted.
     - Check if the request has the right algorithm, and use one of the accepted algorithms.
   * - The credentials date does not match with the request date.
     - The short date in the request's date header is not equal with the Authorization header's.
     - Check the authorization header's credential parameter. The date in this parameter should be the same day as the request's date.
   * - The request date is not within the accepted time range.
     - Differential between the request's date and the process date is bigger then the clockSkew.
     - Check the date header if it has the right date and time. The differential between the date and the process date should be less than the clockSkew (900 seconds).
   * - Invalid Escher key.
     - The Escher key is not in the database.
     - Maybe the database not contains the right key, or the key is misspelled.
   * - The signatures do not match.
     - The generated signature does not match with the authorization header's signature.
     - The authorization header's signature element is wrong. Due the sign process the wrong key was inserted           into the authorization header or somehow it has changed after the sign process but before the authentication.