:original_name: dli_02_0191.html

.. _dli_02_0191:

Binding a Queue
===============

Function
--------

This API is used to bind a queue to a created enhanced datasource connection.

URI
---

-  URI format

   POST /v2.0/{project_id}/datasource/enhanced-connections/{connection_id}/associate-queue

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                                                    |
      +===============+===========+========+================================================================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`.                                  |
      +---------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | connection_id | Yes       | String | Connection ID. Identifies the UUID of a datasource connection. Set the value to the connection ID returned by :ref:`Creating an Enhanced Datasource Connection <dli_02_0187>`. |
      +---------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +------------------------+-----------+------------------+--------------------------------------------------------------------------------+
   | Parameter              | Mandatory | Type             | Description                                                                    |
   +========================+===========+==================+================================================================================+
   | queues                 | No        | Array of Strings | List of queue names that are available for datasource connections.             |
   +------------------------+-----------+------------------+--------------------------------------------------------------------------------+
   | elastic_resource_pools | No        | Array of Strings | Elastic resource pools you want to bind to the enhanced datasource connection. |
   +------------------------+-----------+------------------+--------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Type    | Description                                                                                                                 |
   +============+=========+=============================================================================================================================+
   | is_success | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | String  | System prompt. If execution succeeds, the message may be left blank.                                                        |
   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

.. code-block::

   {
     "queues": [
       "q1",
       "q2"
     ]
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "associated peer connection for queues: {q1,q2}."
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0191__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0191__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== =========================
   Status Code Description
   =========== =========================
   200         Resource bound succeeded.
   400         Request error.
   500         Internal service error.
   =========== =========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
