Specification
=============

This document defines the Escher HTTP request signing framework.

Abstract
--------

The Escher HTTP request signing framework enables a third-party client
to securely access an HTTP service. It defines the creation of an
authorization header, and includes message integrity checking, and
can prevent replaying messages. Designed to be used with the HTTPS
protocol.

The framework is based on `Amazon's AWS4 authentication <http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html>`_,
and compatible with Amazon services using their AWS4 protocol.

Status of This Memo
-------------------

This is a work in progress specification.

Copyright Notice
----------------

Copyright (c) 2014 Emarsys. All rights reserved.

1. Introduction
---------------

The Escher HTTP request signing framework is intended to provide a
secure way for clients to sign HTTP requests, and servers to check
the integrity of these messages. The goal of the protocol is
to introduce an authentication solution for REST API services, that
is more secure than the currently available protocols.

`RFC 2617 (HTTP Authentication) <http://tools.ietf.org/html/rfc2617>`_
defines Basic and Digest Access Authentication. They're widely used,
but Basic Access Authentication doesn't encrypts the secret and doesn't
add integrity checks to the requests. Digest Access Authentication
sends the secret encrypted, but the algorithm with creating a checksum
with a nonce and using md5 should not considered highly secure these
days, and as with Basic Access Authentication, there's no way to check
the integrity of the message.

`RFC 6749 (OAuth 2.0 Authorization) <http://tools.ietf.org/html/rfc6749>`_
enables a third-party application to obtain limited access to an HTTP
service, either on behalf of a resource owner by orchestrating an approval
interaction between the resource owner and the HTTP service, or by allowing
the third-party application to obtain access on its own behalf. This is
not helpful for a machine-to-machine communication situation, like a
REST API authentication, because typically there's no third-party user
involved. Additionally, after a token is obtained from the authorization
endpoint, it is used with no encryption, and doesn't provides integration
checking, or prevents repeating messages. And OAuth 2.0 is a stateful
protocol, needs a database to store the tokens for client sessions.

Amazon, and other service providers created protocols addressing these
issues, however there are no public standard with open source
implementations available from them. As Escher is based on a publicly
documented, widely, in-the-wild used protocol, the specification
not includes novelty techniques.

2. Signing an HTTP request
--------------------------

Escher defines a stateless signature generation mechanism. The signature
is calculated from the key parts of the HTTP request, a service identifier
string called credential scope, and a client key and secret.

The signature generation steps are: canonicalizing the request, creating
a string to calculate the signature, and adding the signature to the
original request.

Canonicalizing the request
--------------------------

In order to calculate a checksum from the key HTTP request parts, the
HTTP request method, the request URI, the query parts, the headers, and
the request body have to be canonicalized. The output of the
canonicalization step will be a string includes the request parts
separated by ``LF`` (Line feed, '\n', 0x0A, 10 in decimal) characters.
The string will be used to calculate a checksum for the request.

The HTTP method
^^^^^^^^^^^^^^^

The HTTP method defined by `RFC2616 (Hypertext Transfer Protocol) <https://tools.ietf.org/html/rfc2616#section-5.1.1>`_
is case sensitive, and must be available in upper case, no transformation
have to be applied.

The Path
^^^^^^^^

The path is the absolute path of the URL. Starts with a slash (``/``)
character, and not includes the query part (and the question mark).

Escher follows the rules defined by `RFC3986 (Uniform Resource Identifier) <http://tools.ietf.org/html/rfc3986>`_
to normalize the path. Basically it means:

 * Convert relative paths to absolute, remove redundant path components
 * URI-encode each path components

   * the "reserved characters" defined by `RFC3986 (Uniform Resource Identifier) <http://tools.ietf.org/html/rfc3986>`_ have to be kept as they are (no encoding applied)
   * all other characters have to be percent encoded, including ``SPACE`` (to ``%20``, instead of ``+``)
   * non-ASCII, UTF-8 characters should be percent encoded to 2 or more pieces (``á`` to ``%C3%A1``)
   * percent encoded hexadecimal numbers have to be upper cased (eg: ``a%c2%b1b`` to ``a%C2%B1b``)

 * Normalize empty paths to ``/``

The Query String
^^^^^^^^^^^^^^^^

`RFC3986 (Uniform Resource Identifier) <http://tools.ietf.org/html/rfc3986>`_ should provide guidance for
canonicalization of the query string, but here's the complete list of the rules have to be applied:

 * URI-encode each query parameter names and values

   * the "reserved characters" defined by `RFC3986 (Uniform Resource Identifier) <http://tools.ietf.org/html/rfc3986>`_ have to be kept as they are (no encoding applied)
   * all other characters have to be percent encoded, including ``SPACE`` (to ``%20``, instead of ``+``)
   * non-ASCII, UTF-8 characters should be percent encoded to 2 or more pieces (``á`` to ``%C3%A1``)
   * percent encoded hexadecimal numbers have to be upper cased (eg: ``a%c2%b1b`` to ``a%C2%B1b``)

 * Normalize empty query strings to empty string
 * Sort query parameters by the encoded parameter names (ASCII order)
 * Do not short parameter values if their parameter name is the same (``key=B&key=A`` is a valid output),
   the order of parameters in a URL may be significant (this is not defined by the HTTP standard)
 * Separate parameter names and values by ``=`` signs, include ``=`` for empty values, too
 * Separate parameters by ``&``

The Headers
^^^^^^^^^^^

To canonicalize the headers, these rules have to be followed:

 * Lower case the header names
 * Separate header names and values by a ``:``, with no spaces
 * Sort header names to alphabetical order (ASCII)
 * Group headers with the same names into one header, and separate their values by commas, without sorting
 * Trim header values, keep all the spaces between quote characters (``"``)
