Escher - stateless HTTP request signing
=======================================

Escher is a **stateless HTTP request signing spec** to allow secure authorization and request validation. It
adds an additional security layer and an authentication layer over HTTPS. The algorithm is based on
`Amazon's AWS4 authentication <http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html>`_.
The goal was implementing a protocol where request's integrity is validated, and impossible to modify without
knowing the secret.

Escher is great for creating **secure REST API servers**, both the client side **request signing** process, and
the server side **validation** and **authentication** processes are implemented. The protocol also provides a
solution for **presigning URLs** with expiration time.

The status is **work in progress**. We're working on the documentation and finishing the first implementations.

Your help will be *much welcomed*, we are especially interested in **code reviews and audits**,
**new library implementations in different languages**, **writing the documentation** and **promotion**.

Feel free to join and discuss at Escher's `general mailing list <https://groups.google.com/forum/#!forum/escherauth>`_,
and follow our Twitter user `@escherauth <https://twitter.com/escherauth>`.

Contents
--------

.. toctree::
   :titlesonly:

   self
   specification
   configuration
   use_cases
   implementations
   implementing