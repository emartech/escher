.. Escher documentation master file, created by
   sphinx-quickstart on Sun Sep 21 21:05:03 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Escher's documentation!
==================================

Escher is a **stateless HTTP request signing library** to allow secure authorization and request validation. It is a generalized version of Amazon's AWS Signature Version algorithm. The goal was implementing a protocol where every request is validated, and impossible to modify without knowing the secret.

Escher is great for creating secure REST API servers, both the **request signing**, and **validation** are implemented. The protocol also provides a solution for **presigning URLs** with expiration.

The status is **work in progress**.

Your help will be *much welcomed*, we are especially interested in **code reviews**, **library implementations in different languages**, **writing the documentation** and **promotion**.

Feel free to join and discuss at Escher's `general mailing list <https://groups.google.com/forum/#!forum/escherauth>`_.

**Implementations**:
 * **PHP** - source code: EscherPHP_ - composer package: `emartech/escher <https://packagist.org/packages/emartech/escher>`_
 * **Ruby** - source code: EscherRuby_ - gem: `escher <https://rubygems.org/gems/escher>`_
 * **NodeJS** - source code: EscherJS_ - npm package: `escher-auth <https://www.npmjs.org/package/escher-auth>`_

Contents:

.. toctree::
   :maxdepth: 2

.. _EscherPHP: https://github.com/emartech/escher-php
.. _EscherRuby: https://github.com/emartech/escher-ruby
.. _EscherJS: https://github.com/emartech/escher-js

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

