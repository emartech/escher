Escher - stateless HTTP request signing
=======================================

Escher is a **stateless HTTP request signing spec** to allow secure authorization and request validation.
The algorithm is based on `Amazon's AWS4 authentication <http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html>`_.
The goal was implementing a protocol where every request is validated, and impossible to modify without
knowing the secret.

Escher is great for creating **secure REST API servers**, both the **request signing**, and **validation** are
implemented. The protocol also provides a solution for **presigning URLs** with expiration.

The status is **work in progress**.

Your help will be *much welcomed*, we are especially interested in **code reviews and audits**,
**new library implementations in different languages**, **writing the documentation** and **promotion**.

Feel free to join and discuss at Escher's `general mailing list <https://groups.google.com/forum/#!forum/escherauth>`_.

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