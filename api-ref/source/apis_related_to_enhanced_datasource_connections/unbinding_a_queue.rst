:original_name: dli_02_0192.html

.. _dli_02_0192:

Unbinding a Queue
=================

Function
--------

This API is used to unbind a queue from an enhanced datasource connection.

URI
---

-  URI format

   POST /v2.0/{project_id}/datasource/enhanced-connections/{connection_id}/disassociate-queue

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | connection_id | Yes       | String | Connection ID. Identifies the UUID of a datasource connection.                                                                                |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------+-----------+-----------------+--------------------------------------------------------------------+
   | Parameter | Mandatory | Type            | Description                                                        |
   +===========+===========+=================+====================================================================+
   | queues    | No        | Array of String | List of queue names that are available for datasource connections. |
   +-----------+-----------+-----------------+--------------------------------------------------------------------+

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

Unbind queues **q1** and **q2** from enhanced datasource connections.

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
     "message": "Disassociated peer connection for queues:{q1,q2}."
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0192__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0192__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   ============ ===========================
   Status Codes Description
   ============ ===========================
   200          Resource unbound succeeded.
   400          Request error.
   500          Internal service error.
   ============ ===========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
