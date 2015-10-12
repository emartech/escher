# Escher

Escher is a **stateless HTTP request signing library** to allow secure authorization and request validation. It is a generalized version of Amazon's AWS Signature Version algorithm. The goal was implementing a protocol where every request is validated, and impossible to modify without knowing the secret.

Escher is great for creating secure REST API servers, both the **request signing**, and **validation** are implemented. The protocol also provides a solution for **presigning URLs** with expiration.

The status is **work in progress**, however we have implementations in 7 programming languages.

Your help will be *much welcomed*, we are especially interested in **code reviews**, **library implementations in different languages**, **writing the documentation** and **promotion**.

Feel free to join and discuss at Escher's [general mailing list](https://groups.google.com/forum/#!forum/escherauth).

**Implementations**:
 * **PHP** - source code: [EscherPHP](https://github.com/emartech/escher-php) - composer package: [emartech/escher](https://packagist.org/packages/emartech/escher)
 * **Ruby** - source code: [EscherRuby](https://github.com/emartech/escher-ruby) - gem: [escher](https://rubygems.org/gems/escher)
 * **NodeJS** - source code: [EscherJS](https://github.com/emartech/escher-js) - npm package: [escher-auth](https://www.npmjs.org/package/escher-auth)
