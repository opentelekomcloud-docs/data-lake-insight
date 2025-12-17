:original_name: dli_02_0021.html

.. _dli_02_0021:

Querying Job Status
===================

Function
--------

This API is used to query the status of a submitted job.

URI
---

-  URI format

   GET /v1.0/{project_id}/jobs/{job_id}/status

-  Parameter descriptions

   .. table:: **Table 1** URI parameters

      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                        |
      +=================+=================+=================+====================================================================================================================================+
      | project_id      | Yes             | String          | **Definition**                                                                                                                     |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | Project ID, which is used for resource isolation. For how to obtain a project ID, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | Example: **48cc2c48765f481480c7db940d6409d1**                                                                                      |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | **Constraints**                                                                                                                    |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | None                                                                                                                               |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | **Range**                                                                                                                          |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | The value can contain 1 to 64 characters. Only letters and digits are allowed.                                                     |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | **Default Value**                                                                                                                  |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | None                                                                                                                               |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------+
      | job_id          | Yes             | String          | **Definition**                                                                                                                     |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | Job ID. For details about how to obtain a job ID, see :ref:`Querying Job Details <dli_02_0022>`.                                   |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | Example: **6d2146a0-c2d5-41bd-8ca0-ca9694ada992**                                                                                  |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | **Constraints**                                                                                                                    |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | None                                                                                                                               |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | **Range**                                                                                                                          |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | The value can contain 1 to 64 characters. Only letters and digits are allowed.                                                     |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | **Default Value**                                                                                                                  |
      |                 |                 |                 |                                                                                                                                    |
      |                 |                 |                 | None                                                                                                                               |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response parameters

   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Type                  | Description                                                                                                                                                   |
   +========================+=======================+===============================================================================================================================================================+
   | is_success             | Boolean               | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Whether the request is successfully executed                                                                                                                  |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **true** indicates that the request is successfully executed.                                                                                                 |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **false** indicates that the request fails to be executed.                                                                                                    |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message                | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | System prompt. If the execution succeeds, this parameter is left blank.                                                                                       |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_id                 | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Job ID                                                                                                                                                        |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_type               | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Job type                                                                                                                                                      |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Options: **DDL**, **DCL**, **IMPORT**, **EXPORT**, **QUERY**, **INSERT**, **DATA_MIGRATION**, **UPDATE**, **DELETE**, **RESTART_QUEUE**, and **SCALE_QUEUE**. |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | -  **DDL**: jobs that create, modify, and delete metadata files                                                                                               |
   |                        |                       | -  **DCL**: jobs that grant and revoke permissions                                                                                                            |
   |                        |                       | -  **IMPORT**: jobs that import external data into the database                                                                                               |
   |                        |                       | -  **EXPORT**: jobs that export data to an external database                                                                                                  |
   |                        |                       | -  **QUERY**: jobs that run query statements                                                                                                                  |
   |                        |                       | -  **INSERT**: jobs that add new data to tables                                                                                                               |
   |                        |                       | -  **DATA_MIGRATION**: jobs that migrate data across sources                                                                                                  |
   |                        |                       | -  **UPDATE**: jobs that update table data                                                                                                                    |
   |                        |                       | -  **DELETE**: jobs that delete data from specified tables                                                                                                    |
   |                        |                       | -  **RESTART_QUEUE**: jobs that restart specified queues                                                                                                      |
   |                        |                       | -  **SCALE_QUEUE**: jobs that scale in or out specified queues                                                                                                |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_mode               | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Job execution mode                                                                                                                                            |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | -  **async**: asynchronous                                                                                                                                    |
   |                        |                       | -  **sync**: synchronous                                                                                                                                      |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name             | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Name of the queue where the job is submitted                                                                                                                  |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | owner                  | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | User who submits a job                                                                                                                                        |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_time             | Long                  | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Time when a job is started. The timestamp is in milliseconds.                                                                                                 |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | duration               | Long                  | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Job running duration, in milliseconds.                                                                                                                        |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | status                 | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Job status                                                                                                                                                    |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | -  **RUNNING**: The job is running.                                                                                                                           |
   |                        |                       | -  **SCALING**: The job is modifying specifications.                                                                                                          |
   |                        |                       | -  **LAUNCHING**: The job is being submitted.                                                                                                                 |
   |                        |                       | -  **FINISHED**: The job has been completed.                                                                                                                  |
   |                        |                       | -  **FAILED**: The job has failed.                                                                                                                            |
   |                        |                       | -  **CANCELLED**: The job has been canceled.                                                                                                                  |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | input_row_count        | Long                  | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Number of records scanned during the Insert job execution                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | bad_row_count          | Long                  | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Number of error records scanned during the Insert job execution                                                                                               |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | input_size             | Long                  | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Size of files scanned during job execution, in bytes                                                                                                          |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | result_count           | Integer               | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Total number of records returned by the current job or total number of records inserted by the Insert job.                                                    |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database_name          | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Name of the database where the target table resides. **database_name** is valid only for jobs of the **IMPORT** **EXPORT**, and **QUERY** types.              |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name             | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Name of the target table. **table_name** is valid only for jobs of the **IMPORT** **EXPORT**, and **QUERY** types.                                            |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | detail                 | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | JSON string for information about related columns                                                                                                             |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Default Value**                                                                                                                                             |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | statement              | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | SQL statements of a job                                                                                                                                       |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags                   | Array of objects      | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Job tags. For details, see :ref:`Table 3 <dli_02_0021__table9391124139>`.                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_conf              | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Spark parameters configured when a user submits a SQL job.                                                                                                    |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | You can set the parameter in key-value pairs. For details about the supported configuration items, see :ref:`Table 3 <dli_02_0102__table334825142314>`.       |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | result_format          | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Storage format of job results                                                                                                                                 |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | Currently, only CSV is supported.                                                                                                                             |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | result_path            | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | OBS path of job results                                                                                                                                       |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | execution_details_path | String                | **Definition**                                                                                                                                                |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | OBS path of the job query execution plan.                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | **Range**                                                                                                                                                     |
   |                        |                       |                                                                                                                                                               |
   |                        |                       | None                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0021__table9391124139:

.. table:: **Table 3** tags parameters

   +-----------------+-----------------+-----------------+-----------------+
   | Parameter       | Mandatory       | Type            | Description     |
   +=================+=================+=================+=================+
   | key             | Yes             | String          | **Definition**  |
   |                 |                 |                 |                 |
   |                 |                 |                 | Tag key         |
   |                 |                 |                 |                 |
   |                 |                 |                 | **Range**       |
   |                 |                 |                 |                 |
   |                 |                 |                 | None            |
   +-----------------+-----------------+-----------------+-----------------+
   | value           | Yes             | String          | **Definition**  |
   |                 |                 |                 |                 |
   |                 |                 |                 | Tag value       |
   |                 |                 |                 |                 |
   |                 |                 |                 | **Range**       |
   |                 |                 |                 |                 |
   |                 |                 |                 | None            |
   +-----------------+-----------------+-----------------+-----------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "job_id": "208b08d4-0dc2-4dd7-8879-ddd4c020d7aa",
     "job_type": "QUERY",
     "job_mode":"async",
     "queue_name": "default",
     "owner": "test",
     "start_time": 1509335108918,
     "duration": 2523,
     "status": "FINISHED",
     "input_size": 22,
     "result_count": 4,
     "database_name":"dbtest",
     "table_name":"tbtest",
     "detail": "{\"type\":\"struct\",\"fields\":[{\"name\":\"id\",\"type\":\"integer\",\"nullable\":true,\"metadata\":{}},{\"name\":\"name\",\"type\":\"string\",\"nullable\":true,\"metadata\":{}}]}",
     "statement": "select * from t1"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0021__tb12870f1c5f24b27abd55ca24264af36>` describes status codes.

.. _dli_02_0021__tb12870f1c5f24b27abd55ca24264af36:

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
