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

.. _to-api-cachegroups-id-queue_update:

***********************************
``cachegroups/{{ID}}/queue_update``
***********************************

``POST``
========
:term:`Queue` or "dequeue" updates for all of a :ref:`Cache Group's servers <cache-group-servers>`, limited to a specific CDN.

:Auth. Required: Yes
:Roles Required: "admin" or "operations"
:Permissions Required: CACHE-GROUP:READ, CDN:READ, SERVER:READ, SERVER:QUEUE
:Response Type:  Object

Request Structure
-----------------
.. table:: Request Path Parameters

	+------+------------------------------------------------------------------------------------------------------------+
	| Name | Description                                                                                                |
	+======+============================================================================================================+
	| ID   | The :ref:`cache-group-id` of the :term:`Cache Group` for which updates are being :term:`Queue`\ d/dequeued |
	+------+------------------------------------------------------------------------------------------------------------+

:action: The action to perform; one of "queue" or "dequeue"
:cdn:    The full name of the CDN in need of :term:`Queue Updates`, or a "dequeue" thereof\ [#required]_
:cdnId:  The integral, unique identifier for the CDN in need of :term:`Queue Updates`, or a "dequeue" thereof\ [#required]_

.. code-block:: http
	:caption: Request Example

	POST /api/4.0/cachegroups/8/queue_update HTTP/1.1
	Host: trafficops.infra.ciab.test
	User-Agent: curl/7.47.0
	Accept: */*
	Cookie: mojolicious=...
	Content-Length: 42
	Content-Type: application/json

	{"action": "queue", "cdn": "CDN-in-a-Box"}


Response Structure
------------------
:action:         The action processed, one of "queue" or "dequeue"
:cachegroupId:   An integer that is the :ref:`cache-group-id` of the :term:`Cache Group` for which :term:`Queue Updates` was performed or cleared
:cachegroupName: The name of the :term:`Cache Group` for which updates were queued/dequeued
:cdn:            The name of the CDN to which the queue/dequeue operation was restricted
:serverNames:    An array of the (short) hostnames of the :ref:`Cache Group's servers <cache-group-servers>` which are also assigned to the CDN specified in the ``"cdn"`` field

.. code-block:: http
	:caption: Response Example

	HTTP/1.1 200 OK
	Access-Control-Allow-Credentials: true
	Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept, Set-Cookie, Cookie
	Access-Control-Allow-Methods: POST,GET,OPTIONS,PUT,DELETE
	Access-Control-Allow-Origin: *
	Content-Type: application/json
	Set-Cookie: mojolicious=...; Path=/; Expires=Mon, 18 Nov 2019 17:40:54 GMT; Max-Age=3600; HttpOnly
	Whole-Content-Sha512: UAcP7LrflU1RnfR4UqbQrJczlk5rkrcLOtTXJTFvIUXxK1EklZkHkE4vewjDaVIhJJ6YQg8jmPGQpr+x1RHabw==
	X-Server-Name: traffic_ops_golang/
	Date: Wed, 14 Nov 2018 20:19:46 GMT
	Content-Length: 115

	{ "response": {
		"cachegroupName": "test",
		"action": "queue",
		"serverNames": [
			"foo"
		],
		"cdn": "CDN-in-a-Box",
		"cachegroupID": 8
	}}

.. [#required] Either 'cdn' or 'cdnID' *must* be in the request data (but not both).
