:original_name: dli_02_0108.html

.. _dli_02_0108:

Previewing Table Content
========================

Function
--------

This API is used to preview the first 10 rows in a table.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/tables/{table_name}/preview

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database where the table to be previewed resides.                                                                                 |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | table_name    | Yes       | String | Name of the table to be previewed.                                                                                                            |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------+-----------+--------+--------------------------------------------------------------------------------------------+
      | Parameter | Mandatory | Type   | Description                                                                                |
      +===========+===========+========+============================================================================================+
      | mode      | No        | String | Preview table mode. The options are **SYNC** and **ASYNC**. The default value is **SYNC**. |
      +-----------+-----------+--------+--------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      GET /v1.0/{project_id}/databases/{database_name}/tables/{table_name}/preview?mode=\ *{previewMode}*

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                                                                                       |
   +============+===========+==================+===================================================================================================================+
   | is_success | No        | Boolean          | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | schema     | No        | Array of objects | Column name and type of a table.                                                                                  |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | rows       | No        | Array of objects | Previewed table content.                                                                                          |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

The following is an example of a successful response in synchronous mode:

.. code-block::

   {
          "is_success": true,
          "message": "",
          "schema": [
              {
                "id": "int"
              },
              {
                "name": "string"
              },
              {
                "address": "string"
              }
           ],
           "rows": [
              [
                  "1",
                  "John",
                  "xxx"
              ],
              [
                  "2",
                  "Lily",
                  "xxx"
              ]
          ]
       }

.. note::

   In asynchronous request mode, a job ID is returned. You can obtain the preview information based on the job ID.

Status Codes
------------

:ref:`Table 4 <dli_02_0108__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0108__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ========================
   Status Code Description
   =========== ========================
   200         The query is successful.
   400         Request error.
   500         Internal service error.
   =========== ========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
