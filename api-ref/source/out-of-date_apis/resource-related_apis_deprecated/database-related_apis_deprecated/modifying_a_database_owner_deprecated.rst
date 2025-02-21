:original_name: dli_02_0164.html

.. _dli_02_0164:

Modifying a Database Owner (Deprecated)
=======================================

Function
--------

This API is used to modify the owner of a database.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   PUT /v1.0/{project_id}/databases/{database_name}/owner

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of a database.                                                                                                                           |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------+-----------+--------+-------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                   |
   +===========+===========+========+===============================================================================+
   | new_owner | Yes       | String | Name of the new owner. The new user must be a sub-user of the current tenant. |
   +-----------+-----------+--------+-------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Change the owner of the database to **scuser1**.

.. code-block::

   {
       "new_owner": "scuser1"
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": ""
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0164__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0164__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ===========================================
   Status Code Description
   =========== ===========================================
   200         The modification operations are successful.
   400         Request error.
   500         Internal service error.
   =========== ===========================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
