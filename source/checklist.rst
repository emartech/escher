Checklist for Auditing
======================

Are you ready with your implementation, and would like to double check if it works
well and secure? First, you should run the test package, but we are also providing
you this checklist you can go through and audit your code.

Request Signing
---------------

**Request canonization**

It has to canonicalize the request properly.

 * Is it correctly parsing *request URI* into *path* and *query*?
 * Is it upper casing the HTTP method?
 * Is it normalizing paths correctly?
 * Is it normalizing query string correctly?
    * Must URL encode query keys
    * Must URL encode query values
    * Must handle empty value correctly
    * Must sort keys
 * Is it normalizing headers correctly?
    * Must lowercase header names
    * Must trim value beginning
    * Must trim value ending
    * Must keep spaces when they are inside quotes
 * Is it creating the signed headers list correctly?
    * Must only include actually signed headers
    * Must merge duplicate key values into a single one separated by commas
    * Must sort by header key/name
 * Is it normalizing signed headers list correctly?
    * Must lowercase headers keys/names
    * Must sort headers
    * Must join headers with a semicolon
 * Is it hashing the payload correctly?
    * Must be lowercase hexadecimal representation
 * Is it correctly formatting the canonicalized request?
 	* Must have uppercase HTTP verb on the 1st line
 	* Must have the path in the 2nd line
 	* Must have the canonicalized query string on the 3rd line
 	* Must include headers
 	* Must include an empty line after the header section
 	* Must include the signed headers
 	* Must include the payload hash
 * Is it correctly formatting the string to sign?
    * Must format the algorithm description correctly
       * Must include a vendor prefix
       * Must include the constant "HMAC"
       * Must include the uppercase algorithm name
       * Must be separated by dashes ([PREFIX]-HMAC-[ALGO])
    * Must format the date correctly
       * Must be in ISO8601 format
       * Must be in GMT/UTC timezone
    * Must prefix credential scope with date (day) and a slash
    * Must have the following structure
       * Must include the algorithm description on the 1st line
       * Must include the date on the 2nd line
       * Must include the credential scope on the 3rd line
       * Must include the hash of the canonical request on the 4th line
 * Is it calculating the signature correctly?
 	* Must calculate a signing key correctly
 	   * Must hash combine the credential scope
 	   * Must NOT use hexdigest
 	* Must calculate signature using HMAC and the defined hash algorithm
 * Is it assembling the Authentication header correctly?
    * Must use the correct algorithm format
    * Must use the correct credential format
    * Must use the correct signed headers format
    * Must use the correct signature format
    * Must have the following structure
       * Must include the algorithm description
       * Must have a space after algorithm description
       * Must state credential scope
       * Must have a comma and a space after credential scope
       * Must state signed headers
       * Must have a comma and a space after signed headers
       * Must state signature
 * Is the implementation consistent?
 	* Regarding date handling
 	* Regarding algorithm
 	* Regarding credential scope
 	* Regarding full credential scope

Client must provide default constants and hide request canonization and signature.

Server must check signatures correctly.



