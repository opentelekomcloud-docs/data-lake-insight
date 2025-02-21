:original_name: dli_02_0035.html

.. _dli_02_0035:

Deleting a Table (Deprecated)
=============================

Function
--------

This API is used to delete a specified table.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   DELETE /v1.0/{project_id}/databases/{database_name}/tables/{table_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database where the table to be deleted resides.                                                                                   |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | table_name    | Yes       | String | Name of the table to be deleted.                                                                                                              |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------+-----------+---------+----------------------------------------------------------------------------------------------------------------------------------+
      | Parameter | Mandatory | Type    | Description                                                                                                                      |
      +===========+===========+=========+==================================================================================================================================+
      | async     | No        | Boolean | Specifies whether to delete the database in asynchronous mode. The value can be **true** or **false**. Default value: **false**. |
      +-----------+-----------+---------+----------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      DELETE /v1.0/{project_id}/databases/{database_name}/tables/{table_name}?async=\ *{is_async*}

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                 |
   +=================+=================+=================+=============================================================================================================================+
   | is_success      | No              | Boolean         | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message         | No              | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | job_mode        | No              | String          | Job execution mode. The options are as follows:                                                                             |
   |                 |                 |                 |                                                                                                                             |
   |                 |                 |                 | -  **async**: asynchronous                                                                                                  |
   |                 |                 |                 | -  **sync**: synchronous                                                                                                    |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
    "is_success": true,
    "message": ""
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0035__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0035__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

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
