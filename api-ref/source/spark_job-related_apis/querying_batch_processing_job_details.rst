:original_name: dli_02_0126.html

.. _dli_02_0126:

Querying Batch Processing Job Details
=====================================

Function
--------

This API is used to query details about a batch processing job based on the job ID.

URI
---

-  URI format

   GET /v2.0/{project_id}/batches/{batch_id}

-  Parameter descriptions

   .. table:: **Table 1** URI parameters

      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                          |
      +=================+=================+=================+======================================================================================================================================================================+
      | project_id      | Yes             | String          | **Definition**                                                                                                                                                       |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | Project ID, which is used for resource isolation. For how to obtain a project ID, see :ref:`Obtaining a Project ID <dli_02_0183>`.                                   |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | Example: **48cc2c48765f481480c7db940d6409d1**                                                                                                                        |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | **Constraints**                                                                                                                                                      |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | None                                                                                                                                                                 |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | **Range**                                                                                                                                                            |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | The value can contain up to 64 characters. Only letters and digits are allowed.                                                                                      |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | **Default Value**                                                                                                                                                    |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | None                                                                                                                                                                 |
      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | batch_id        | Yes             | String          | **Definition**                                                                                                                                                       |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | ID of a batch processing job. For details about how to obtain an ID, see **appId** in the response parameters in :ref:`Listing Batch Processing Jobs <dli_02_0125>`. |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | Example: **0a324461-d9d9-45da-a52a-3b3c7a3d809e**                                                                                                                    |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | **Constraints**                                                                                                                                                      |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | The parameter value must be a string matching the regular expression **^[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}$**.              |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | **Range**                                                                                                                                                            |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | None                                                                                                                                                                 |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | **Default Value**                                                                                                                                                    |
      |                 |                 |                 |                                                                                                                                                                      |
      |                 |                 |                 | None                                                                                                                                                                 |
      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response parameters

   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                              |
   +=================+=================+==================+==========================================================================================================================================================+
   | id              | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | ID of a batch processing job, which is in the universal unique identifier (UUID) format                                                                  |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | appId           | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Back-end application ID of a batch processing job.                                                                                                       |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | name            | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Name of a batch processing job.                                                                                                                          |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | owner           | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Owner of a batch processing job.                                                                                                                         |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | proxyUser       | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Proxy user (resource tenant) to which a batch processing job belongs.                                                                                    |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | state           | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Status of a batch processing job                                                                                                                         |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | -  **starting**: A job is being started.                                                                                                                 |
   |                 |                 |                  | -  **running**: A job is being executed.                                                                                                                 |
   |                 |                 |                  | -  **dead**: A session has exited.                                                                                                                       |
   |                 |                 |                  | -  **success**: A session is successfully stopped.                                                                                                       |
   |                 |                 |                  | -  **recovering**: A job is being restored.                                                                                                              |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kind            | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Type of a batch processing job. Only Spark parameters are supported.                                                                                     |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | log             | No              | Array of strings | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Last 10 records of the current batch processing job.                                                                                                     |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sc_type         | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Compute resource type. Currently, the value can be **A**, **B**, or **C**. If the compute resource type is customized, value **CUSTOMIZED** is returned. |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | -  **A**: physical resources, with 8 vCPUs and 32 GB of memory                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  |    -  driverCores: 2;                                                                                                                                    |
   |                 |                 |                  |    -  executorCores: 1;                                                                                                                                  |
   |                 |                 |                  |    -  driverMemory: 7G;                                                                                                                                  |
   |                 |                 |                  |    -  executorMemory: 4G;                                                                                                                                |
   |                 |                 |                  |    -  numExecutor: 6.                                                                                                                                    |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | -  **B**: physical resources, with 16 vCPUs and 64 GB of memory                                                                                          |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  |    -  driverCores: 2;                                                                                                                                    |
   |                 |                 |                  |    -  executorCores: 2;                                                                                                                                  |
   |                 |                 |                  |    -  driverMemory: 7G;                                                                                                                                  |
   |                 |                 |                  |    -  executorMemory: 8G;                                                                                                                                |
   |                 |                 |                  |    -  numExecutor: 7.                                                                                                                                    |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | -  **C**: physical resources, with 32 vCPUs and 128 GB of memory                                                                                         |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  |    -  driverCores: 4;                                                                                                                                    |
   |                 |                 |                  |    -  executorCores: 2;                                                                                                                                  |
   |                 |                 |                  |    -  driverMemory: 15G;                                                                                                                                 |
   |                 |                 |                  |    -  executorMemory: 8G;                                                                                                                                |
   |                 |                 |                  |    -  numExecutor: 14.                                                                                                                                   |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_name    | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Queue where a batch processing job is located.                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue           | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Queue where a batch processing job is located.                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | create_time     | No              | Long             | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Time when a batch processing job is created. The timestamp is in milliseconds.                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | update_time     | No              | Long             | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Time when a batch processing job is updated. The timestamp is in milliseconds.                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | req_body        | No              | String           | **Definition**                                                                                                                                           |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | Request parameter details.                                                                                                                               |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | **Range**                                                                                                                                                |
   |                 |                 |                  |                                                                                                                                                          |
   |                 |                 |                  | None                                                                                                                                                     |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "id": "0a324461-d9d9-45da-a52a-3b3c7a3d809e",
       "appId": "",
       "name": "",
       "owner": "",
       "proxyUser": "",
       "state": "starting",
       "kind":"",
       "log": [
              "stdout: ",
               "stderr: ",
               "YARN Diagnostics: "
       ],
       "sc_type": "A",
       "cluster_name": "test",
       "queue": "test",
       "create_time": 1531906043036,
       "update_time": 1531906043036
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0126__tb12870f1c5f24b27abd55ca24264af36>` describes status codes.

.. _dli_02_0126__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 3** Status codes

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
