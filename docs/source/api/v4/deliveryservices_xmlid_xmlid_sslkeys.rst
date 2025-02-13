..
..
.. Licensed under the Apache License, Version 2.0 (the "License");
.. you may not use this file except in compliance with the License.
.. You may obtain a copy of the License at
..
..     http://www.apache.org/licenses/LICENSE-2.0
..
.. Unless required by applicable law or agreed to in writing, software
.. distributed under the License is distributed on an "AS IS" BASIS,
.. WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
.. See the License for the specific language governing permissions and
.. limitations under the License.
..

.. _to-api-deliveryservices-xmlid-xmlid-sslkeys:

********************************************
``deliveryservices/xmlId/{{XMLID}}/sslkeys``
********************************************

``GET``
=======
Retrieves SSL keys for a :term:`Delivery Service`.

:Auth. Required: Yes
:Roles Required: None
:Permissions Required: DS-SECURITY-KEY:READ, DELIVERY-SERVICE:READ
:Response Type:  Object

Request Structure
-----------------
.. table:: Request Path Parameters

	+-------+------------------------------------------------------+
	|  Name |              Description                             |
	+=======+======================================================+
	| XMLID | The 'xml_id' of the desired :term:`Delivery Service` |
	+-------+------------------------------------------------------+


.. table:: Request Query Parameters

	+---------+----------+-----------------------------------------------------------------------------------------+
	|  Name   | Required |          Description                                                                    |
	+=========+==========+=========================================================================================+
	| version | no       | The version number of the SSL keys to retrieve                                          |
	+---------+----------+-----------------------------------------------------------------------------------------+
	| decode  | no       | If ``true``, the returned keys will be decoded - if ``false``, they will not be decoded |
	+---------+----------+-----------------------------------------------------------------------------------------+

.. caution:: There's almost certainly no good reason to request the private key! Even when "base 64-encoded" do not let **ANYONE** see this who would be unable to request it themselves!

Response Structure
------------------
:businessUnit: An optional field which, if present, contains the business unit entered by the user when generating the SSL certificate\ [1]_
:certificate:  An object containing the actual generated key, certificate, and signature of the SSL keys

	:crt: Base 64-encoded (or not if the ``decode`` query parameter was given and ``true``) certificate for the :term:`Delivery Service` identified by ``deliveryservice``
	:csr: Base 64-encoded (or not if the ``decode`` query parameter was given and ``true``) csr file for the :term:`Delivery Service` identified by ``deliveryservice``
	:key: Base 64-encoded (or not if the ``decode`` query parameter was given and ``true``) private key for the :term:`Delivery Service` identified by ``deliveryservice``

	.. caution:: There's almost certainly no good reason to request the private key! Even when "base 64-encoded" do not let **ANYONE** see this who would be unable to request it themselves!

:cdn:             The CDN of the :term:`Delivery Service` for which the certs were generated
:city:            An optional field which, if present, contains the city entered by the user when generating the SSL certificate\ [1]_
:country:         An optional field which, if present, contains the country entered by the user when generating the SSL certificate\ [1]_
:deliveryservice: The 'xml_id' of the :term:`Delivery Service` for which the certificate was generated
:hostname:        The hostname generated by Traffic Ops that is used as the common name when generating the certificate - this will be a FQDN for DNS :term:`Delivery Services` and a wildcard URL for HTTP :term:`Delivery Services`
:organization:    An optional field which, if present, contains the organization entered by the user when generating certificate\ [1]_
:state:           An optional field which, if present, contains the state entered by the user when generating certificate\ [1]_
:version:         An integer that defines the "version" of the key - which may be thought of as the sequential generation; that is, the higher the number the more recent the key
:expiration:      The expiration date of the certificate for the :term:`Delivery Service` in :rfc:`3339` format
:sans:            The :abbr:`SANs (Subject Alternate Names)` from the SSL certificate.

	.. versionadded:: 4.1

.. code-block:: http
	:caption: Response Example

	HTTP/1.1 200 OK
	Content-Type: application/json

	{ "response": {
		"certificate": {
			"crt": "crt",
			"key": "key",
			"csr": "csr"
		},
		"deliveryservice": "my-ds",
		"cdn": "qa",
		"businessUnit": "CDN_Eng",
		"city": "Denver",
		"organization": "KableTown",
		"hostname": "foober.com",
		"country": "US",
		"state": "Colorado",
		"version": "1",
		"expiration": "2020-08-18T13:53:06Z",
		"sans": ["*.foober.com", "*.foober2.com"]
	}}

``DELETE``
==========
:Auth. Required: Yes
:Roles Required: "admin" or "operations"
:Permissions Required: DS-SECURITY-KEY:DELETE, DELIVERY-SERVICE:READ, DS-SECURITY-KEY:READ, DELIVERY-SERVICE:UPDATE
:Response Type:  Object (string)

Request Structure
-----------------
.. table:: Request Path Parameters

	+-------+----------+-------------------------------------------------------------+
	| Name  | Required | Description                                                 |
	+=======+==========+=============================================================+
	| xmlId | yes      | The :ref:`ds-xmlid` of the desired :term:`Delivery Service` |
	+-------+----------+-------------------------------------------------------------+

.. table:: Request Query Parameters

	+---------+----------+------------------------------------------------------------+
	|   Name  | Required |          Description                                       |
	+=========+==========+============================================================+
	| version | no       | The version number of the SSL keys that shall be retrieved |
	+---------+----------+------------------------------------------------------------+

Response Structure
------------------
.. code-block:: http
	:caption: Response Example

	HTTP/1.1 200 OK
	Access-Control-Allow-Credentials: true
	Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept, Set-Cookie, Cookie
	Access-Control-Allow-Methods: POST,GET,OPTIONS,PUT,DELETE
	Access-Control-Allow-Origin: *
	Content-Encoding: gzip
	Content-Type: application/json
	Set-Cookie: mojolicious=...; Path=/; Expires=Wed, 18 Mar 2020 17:36:10 GMT; Max-Age=3600; HttpOnly
	Whole-Content-Sha512: Pj+zCoOXg19nGNxcSkjib2iDjG062Y3RcEEV+OYnwbGIsLcpa0BKZleY/qJOKT5DkSoX2qQkckUxUqdDxjVorQ==
	X-Server-Name: traffic_ops_golang/
	Date: Wed, 18 Mar 2020 16:36:10 GMT
	Content-Length: 79

	{
		"response": "Successfully deleted ssl keys for demo1"
	}

.. [1] These optional fields will be present in the response if and only if they were specified during key generation; they are optional during key generation and thus cannot be guaranteed to exist or not exist.
