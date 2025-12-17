:original_name: dli_02_0312.html

.. _dli_02_0312:

Previewing SQL Job Query Results
================================

Function
--------

This API is used to view the job execution result after a job is executed using SQL query statements. Currently, you can only query execution results of jobs of the **QUERY** type.

This API can be used to view only the first 1000 result records and does not support pagination query. To view all query results, you need to export the query results first. For details, see :ref:`Exporting Query Results <dli_02_0024>`.

URI
---

-  URI format

   GET /v1.0/{project_id}/jobs/{job_id}/preview

-  Parameter descriptions

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                        |
      +============+===========+========+====================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For how to obtain a project ID, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | String | Job ID                                                                                                                             |
      +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter descriptions

      +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                  |
      +============+===========+========+==============================================================================================================================+
      | page-size  | No        | Long   | Number of result rows. The value ranges from 1 to 1000. The default rate limit is **1000**.                                  |
      +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------+
      | queue-name | No        | String | Name of the execution queue for obtaining job results. If this parameter is not specified, the default system queue is used. |
      +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      GET /v1.0/{project_id}/jobs/{job_id}/preview?page-size=\ *{size}*\ &queue-name=\ *{queue_name}*

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 3** Response parameters

   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                                            |
   +=================+=================+==================+========================================================================================================================================================================+
   | is_success      | No              | Boolean          | Whether the request is successfully executed. **true** indicates that the request is successfully executed.                                                            |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message         | No              | String           | System prompt. If the execution succeeds, this parameter may be left blank.                                                                                            |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_id          | No              | String           | Job ID You can get the value by calling :ref:`Submitting a SQL Job (Recommended) <dli_02_0102>`.                                                                       |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_type        | No              | String           | Job type. Options: **DDL**, **DCL**, **IMPORT**, **EXPORT**, **QUERY**, **INSERT**, **DATA_MIGRATION**, **UPDATE**, **DELETE**, **RESTART_QUEUE** and **SCALE_QUEUE**. |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | Currently, you can only query execution results of jobs of the **QUERY** type.                                                                                         |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **DDL**: jobs that create, modify, and delete metadata files                                                                                                        |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **DCL**: jobs that grant and revoke permissions                                                                                                                     |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  |    When **job_type** is set to **DCL**, the operation is synchronous.                                                                                                  |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **IMPORT**: jobs that import external data into the database                                                                                                        |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **EXPORT**: jobs that export data to an external database                                                                                                           |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **QUERY**: jobs that run query statements                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **INSERT**: jobs that add new data to tables                                                                                                                        |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **DATA_MIGRATION**: data migration jobs                                                                                                                             |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **UPDATE**: jobs that update table data                                                                                                                             |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **DELETE**: jobs that delete data from tables                                                                                                                       |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **RESTART_QUEUE**: jobs that restart specified queues                                                                                                               |
   |                 |                 |                  |                                                                                                                                                                        |
   |                 |                 |                  | -  **SCALE_QUEUE**: jobs that scale in or out specified queues                                                                                                         |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | row_count       | No              | Integer          | Total number of job results.                                                                                                                                           |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | input_size      | No              | Long             | Amount of data scanned during job execution.                                                                                                                           |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema          | No              | Array of Map     | Name and type of the job result column.                                                                                                                                |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rows            | No              | Array of objects | Job results set.                                                                                                                                                       |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "job_id": "ead0b276-8ed4-4eb5-b520-58f1511e7033",
     "job_type": "QUERY",
     "row_count": 1,
     "input_size": 74,
     "schema": [
       {
         "c1": "int"
       },
       {
         "c2": "string"
       }
     ],
     "rows": [
       [
         23,
         "sda"
       ]
     ]
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0312__tb12870f1c5f24b27abd55ca24264af36>` describes status codes.

.. _dli_02_0312__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ========================
   Status Code Description
   =========== ========================
   200         The query is successful.
   400         Request error.
   500         Internal server error.
   =========== ========================

Error Codes
-----------

If an error occurs when this API is called, the system does not return the result similar to the preceding example, but returns an error code and error message. For details, see :ref:`Error Codes <dli_02_0056>`.
