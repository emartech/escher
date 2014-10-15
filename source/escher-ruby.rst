EscherRuby
==========

 * Source code: `EscherRuby <https://github.com/emartech/escher-ruby>`_
 * gem: `escher <https://rubygems.org/gems/escher>`_

Installation
------------

We are supporting RubyGems as a package manager. Ruby 1.9 and newer versions are shipped with
RubyGems built-in, so it should be available on your system by default. You
might need an update, please check the instructions on the
`RubyGem's Downloads page <https://rubygems.org/pages/download>`_ for more
information.

If RubyGems is installed, you can install the *escher* gem by:

.. code-block:: bash

   gem install escher

Usage
-----

From Ruby 1.9, your installed gems are autoloaded automatically, you can
start using Escher after installation.

The library has 3 interfaces you can call. You can sign an HTTP request, you can presign an URL
and you can validate a signed HTTP request or presigned URL (with the same method).

Signing a request
^^^^^^^^^^^^^^^^^

Escher works by calculating a cryptographic signature of your request, and adding it (and other authentication
information) to the request.
Usually you will want to add the authentication information by appending extra headers to it.
Let's say you want to send a signed POST request to http://example.com/ using the Guzzle\Http library:

.. code-block:: ruby

   require 'escher'
   require 'net/http'

   escher = Escher.new('example/credential/scope', {})
   client = {api_key_id: 'YOUR ACCESS KEY ID', api_secret: 'YOUR SECRET'}

   url = URI.parse('http://example.com/')
   req = Net::HTTP::Post.new(url.to_s)
   req.body = '{ "this_is": "a_request_body" }'
   req = escher.sign!(req, client)
   response = Net::HTTP.start(url.host, url.port) {|http|
     http.request(req)
   }

Presigning a URL
^^^^^^^^^^^^^^^^

In some cases you may want to send authenticated requests from a context where you cannot modify the request headers,
e.g. when embedding an API generated iframe.
You can however generate a presigned URL, where the authentication information is added to the query string.

.. code-block:: ruby

   require 'escher'
   require 'net/http'

   escher = Escher.new('example/credential/scope', {})
   client = {api_key_id: 'YOUR ACCESS KEY ID', api_secret: 'YOUR SECRET'}

   presigned_url = escher.generate_signed_url("http://example.com", client)

Validating a request
^^^^^^^^^^^^^^^^^^^^

You can validate a request signed by the methods described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts any kind of object as a key database that implements the ArrayAccess interface. (It also accepts plain arrays, however it is not recommended to use a php array for a database of API secrets - it's just there to ease testing)

.. code-block:: ruby

   TBD
