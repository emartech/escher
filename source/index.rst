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
and follow our Twitter user `@escherauth <https://twitter.com/escherauth>`_.

Announcement
------------

You can check out our Escher announcement was on `NordicAPIs <http://nordicapis.com/events/stockholm-2014-platform-summit/>`_:

.. raw:: html

   <iframe
     frameborder="0"
     webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""
     width="550" height="400"
     src="http://prezi.com/embed/dfbss2mfbpqo/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0#">
   </iframe>


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