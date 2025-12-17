:original_name: dli_02_0107.html

.. _dli_02_0107:

Checking SQL Syntax
===================

Function
--------

This API is used to check the SQL syntax.

URI
---

-  URI format

   POST /v1.0/{project_id}/jobs/check-sql

-  Parameter descriptions

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                            |
   +=================+=================+=================+========================================================================================================================================================================+
   | sql             | Yes             | String          | SQL statement that you want to execute.                                                                                                                                |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | currentdb       | No              | String          | Database where the SQL statement is executed.                                                                                                                          |
   |                 |                 |                 |                                                                                                                                                                        |
   |                 |                 |                 | .. note::                                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                                        |
   |                 |                 |                 |    -  If the SQL statement contains **db_name**, for example, **select \* from db1.t1**, you do not need to set this parameter.                                        |
   |                 |                 |                 |    -  If the SQL statement does not contain **db_name**, the semantics check will fail when you do not set this parameter or set this parameter to an incorrect value. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

.. table:: **Table 3** Response parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                 |
   +=================+=================+=================+=============================================================================================================+
   | is_success      | No              | Boolean         | Whether the request is successfully executed. **true** indicates that the request is successfully executed. |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | message         | No              | String          | System prompt. If the execution succeeds, this parameter may be left blank.                                 |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | job_type        | No              | String          | Job type. Options: **DDL**, **DCL**, **IMPORT**, **EXPORT**, **QUERY**, and **INSERT**.                     |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  **DDL**: jobs that create, modify, and delete metadata files                                             |
   |                 |                 |                 | -  **DCL**: jobs that grant and revoke permissions                                                          |
   |                 |                 |                 | -  **IMPORT**: jobs that import external data into the database                                             |
   |                 |                 |                 | -  **EXPORT**: jobs that export data to an external database                                                |
   |                 |                 |                 | -  **QUERY**: jobs that run query statements                                                                |
   |                 |                 |                 | -  **INSERT**: jobs that add new data to tables                                                             |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Check the syntax of the SQL statement **select \* from t1**.

.. code-block::

   {
      "currentdb": "db1",
      "sql": "select * from t1"
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "the sql is ok",
     "job_type":"QUERY"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0107__tb12870f1c5f24b27abd55ca24264af36>` describes status codes.

.. _dli_02_0107__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ==========================
   Status Code Description
   =========== ==========================
   200         The request is successful.
   400         Request error.
   500         Internal server error.
   =========== ==========================

Error Codes
-----------

If an error occurs when this API is called, the system does not return the result similar to the preceding example, but returns an error code and error message. For details, see :ref:`Error Codes <dli_02_0056>`.
