:original_name: dli_02_0188.html

.. _dli_02_0188:

Deleting an Enhanced Datasource Connection
==========================================

Function
--------

This API is used to delete an enhanced datasource connection.

.. note::

   The connection that is being created cannot be deleted.

URI
---

-  URI format

   DELETE /v2.0/{project_id}/datasource/enhanced-connections/{connection_id}

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

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Type    | Description                                                                                                                 |
   +============+=========+=============================================================================================================================+
   | is_success | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | String  | System message. Value **Deleted** indicates that the operation is successful.                                               |
   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "Deleted"
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0188__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0188__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 3** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Deletion succeeded.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
