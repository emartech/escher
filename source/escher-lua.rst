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
The authenticate function examine the HTTP request and return with an error for the following cases:

 * There is a missing header in the request

.. code-block:: lua

  local authHeader = self:getHeader(request.headers, self.authHeaderName)
  local dateHeader = self:getHeader(request.headers, self.dateHeaderName)
  local hostHeader = self:getHeader(request.headers, "Host")

  if authHeader == nil then
    return self.dropError("Missisng header: " .. self.authHeaderName)
  end

  if dateHeader == nil then
    return self.dropError("Missisng header: " .. self.dateHeaderName)
  end

  if hostHeader == nil then
    return self.dropError("Missisng header: host")
  end
..
 
 * Algorithm prefix is invalid
 
.. code-block:: lua

  if not string.match(authHeader, "(["..self.algoPrefix .."]+)%-HMAC%-") then
    return self.dropError("Algorithm prefix is invalid")
  end
..
 
 * Algorithm is not supported
 
.. code-block:: lua

  if authParts.hashAlgo ~= self.hashAlgo then
    return self.dropError("Only SHA256 and SHA512 hash algorithms are allowed")
  end
..
 
 * Invalid API key
 
.. code-block:: lua

  if apiSecret == nil then
    return self.dropError("Invalid API key")
  end
..
 
 * The request date and the date in the request's credential do not match
 
.. code-block:: lua

  if authParts.shortDate ~= requestDate:fmt("%Y%m%d") then
    return self.dropError("The credential date does not match with the request date")
  end 
..
 
  * The request date is not within the accepted time range
 
.. code-block:: lua

  if not self:isDateWithinRange(requestDate, self.clockSkew) then
    return self.dropError("The request date is not within the accepted time range")
  end
..
 
 * Invalid credentials
 
.. code-block:: lua

  if authParts.credentialScope ~= self.credentialScope then
    return self.dropError("Invalid credentials")
  end
..
 
 * Host header is not signed
 
.. code-block:: lua

  if not string.match(authParts.signedHeaders, "host") then
    return self.dropError("Host header is not signed")
  end
..
 
 * Date header is not signed
 
.. code-block:: lua

  if not string.match(authParts.signedHeaders, "date") then
    return self.dropError("Date header is not signed")
  end
..
 
 * The signatures do not match
 
.. code-block:: lua

  if authParts.signature ~= self:calculateSignature(request) then
    return self.dropError("The signatures do not match")
  end 
..

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
