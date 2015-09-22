EscherLua
========

 * Source code: `EscherLua <https://github.com/emartech/escher-lua>`_

Installation
-----

To install Escher, follow the following steps:

.. code-block:: bash

   sudo apt-get install luajit
   sudo apt-get install luarocks
   sudo luarocks install luafilesystem
   sudo luarocks install busted
   sudo luarocks install rapidjson
   sudo luarocks install luacrypto
   sudo luarocks install date

These steps install every module needs for Escher.

For mac users the steps:

.. code-block:: bash
   
   brew install lua
   brew install cmake
   brew install openssl
   luarocks-5.2 install luafilesystem
   luarocks-5.2 install busted
   luarocks-5.2 install rapidjson
   luarocks-5.2 install date
   luarocks-5.2 install luacrypto OPENSSL_INCDIR=/usr/local/Cellar/openssl/1.0.2c/include

Usage
-----

To load the library, add the Composer autoloader to your code:

.. code-block:: lua

   local Escher = require("lib.escher")

The library has two interfaces you can call. You can validate a signed HTTP request or you can presign a URL.

Presigning a URL
^^^^^^^^^^^^^^^^
In some cases you may want to send authenticated requests from a context where you cannot modify the request headers, e.g. when embedding an API generated iframe. You can however generate a presigned URL, where the authentication information is added to the query string.

.. code-block:: lua

   local escher = Escher:new(getConfigFromTestsuite(test.config))
   local client = {test.config.accessKeyId, test.config.apiSecret}
   
   test.request.url = escher:generatePreSignedUrl(test.request.url, client, test.request.expires)

Validating a request
^^^^^^^^^^^^^^^^^^^^
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

If there is no error in the request the authentication process returns with the given client key. For that you will need a database of the access keys and secrets of your clients. Escher accepts a function as a key database, where you can pass the client key, and it returns the client secret.

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
