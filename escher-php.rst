EscherPHP
=========

 * Source code: `EscherPHP <https://github.com/emartech/escher-php>`_
 * Composer package: `emartech/escher <https://packagist.org/packages/emartech/escher>`_

## Installation

You can install the *emartech-escher* composer package by:

**TBD**

## Usage

The library has 3 interface. You can sign an HTTP request, you can presign an URL and you can validate a
signed HTTP request or presigned URL (with the same method).

Signing a request
-----------------

Escher works by calculating a cryptographic signature of your request, and adding it (and other authentication
information) to the request.
Usually you will want to add the authentication information by appending extra headers to it.
Let's say you want to send a signed POST request to http://example.com/ using the Guzzle\Http library:

    $method = 'POST';
    $url = 'http://example.com';
    $requestBody = '{ "this_is": "a_request_body" }';
    $yourHeaders = array('Content-Type' => 'application/json');

    $headersWithAuthInfo = Escher::create('example/credential/scope')
        ->signRequest('YOUR ACCESS KEY ID', 'YOUR SECRET', $method, $url, $requestBody, $yourHeaders);

    $client = new GuzzleHttp\Client();
    $response = $client->post($url, array(
        'body' => $requestBody,
        'headers' => $headersWithAuthInfo
    ));

Presigning a URL
----------------

In some cases you may want to send authenticated requests from a context where you cannot modify the request headers,
e.g. when embedding an API generated iframe.
You can however generate a presigned URL, where the authentication information is added to the query string.

    $presignedUrl = Escher::create('example/credential/scope')
        ->presignUrl('YOUR ACCESS KEY ID', 'YOUR SECRET', 'http://example.com');


Validating a request
--------------------

You can validate a request signed by the methods described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts any kind of object as a key database that implements the ArrayAccess interface. (It also accepts plain arrays, however it is not recommended to use a php array for a database of API secrets - it's just there to ease testing)

    try {
        $keyDB = new ArrayObject(array(
            'ACCESS KEY OF CLIENT 1'  => 'SECRET OF CLIENT 1',
            'ACCESS KEY OF CLIENT 42' => 'SECRET OF CLIENT 42',
        ));
        Escher::create('example/credential/scope')->validateRequest($keyDB);
    } catch (EscherException $ex) {
        echo 'The validation failed! ' . $ex->getMessage();
    }
