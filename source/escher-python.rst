EscherPython
============

 * source code: `EscherPython <https://github.com/emartech/escherauth-go-example>`_
 * package: `escher <https://pypi.org/project/escherauth-go/>`_

Installation
------------
Use Python 3.6 or above. Create a local envirioment. Use pip to install escher lib of python.
This is a wrapper for the go implementation. This packages requires glibc which makes it unusable with musl libc (e.g.: in alpine linux)

.. code-block:: bash

  pip install escherauth-go


Usage
-----

After install the package it is ready to be use. See below the examples. The request and response type come from Flask package.

Signing a Request
^^^^^^^^^^^^^^^^^

You can validate a request signed by the methods described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts any kind of object as a key database that implements the ArrayAccess interface.

.. code-block:: python

   from flask import Flask, request
   from escherauth_go.escher_validator import EscherValidator
   from escherauth_go.escher_signer import EscherSignerError

   validator = EscherValidator('eu/suite/ems_request',
                               [{'keyId':'EscherExample', 'secret':'Secret', 'acceptOnly':0}],
                               authHeaderName='X-Ems-Auth',
                               dateHeaderName='X-Ems-Date')

   def escher_validate(web_request):
       parsed_url = urlparse(web_request.url)
       url = (parsed_url.path + '?' + parsed_url.query).rstrip('?')
       validator.validateRequest(
           web_request.method,
           url,
           web_request.data.decode('utf-8'),
           web_request.headers
       )

    def validate_request(request):
        try:
            escher_validate(request)
        except EscherValidatorError as e:
            return Response('Authorization required {}'.format(e), HTTPStatus.UNAUTHORIZED)





Signing a Request
^^^^^^^^^^^^^^^^^

You can validate a request signed by the methods described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts any kind of object as a key database that implements the ArrayAccess interface. (It also accepts plain arrays, however it is not recommended to use a php array for a database of API secrets - it's just there to ease testing)

.. code-block:: python

   import requests
   from escherauth_go.escher_signer import EscherSigner,

   signer = EscherSigner('EscherExample', 'TheBeginningOfABeautifulFriendship', 'eu/suite/ems_request')

   def sign_and_send(body):
       try:
           signed_headers = signer.signRequest(
               'POST',
               '/validate_request',
               body,
               {
                   'host': 'localhost:5000'
               })
           signed_headers.update({'Content-Type':'application/json'})
           return requests.post('http://docker.for.mac.localhost:5001/validate_request', data=body, headers= signed_headers)
       except EscherSignerError as e:
           # Handle sign error
