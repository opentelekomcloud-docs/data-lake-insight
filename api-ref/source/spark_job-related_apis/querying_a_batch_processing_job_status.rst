:original_name: dli_02_0127.html

.. _dli_02_0127:

Querying a Batch Processing Job Status
======================================

Function
--------

This API is used to obtain the execution status of a batch processing job.

URI
---

-  URI format

   GET /v2.0/{project_id}/batches/{batch_id}/state

-  Parameter descriptions

   .. table:: **Table 1** URI parameters

      +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter             | Mandatory             | Description                                                                                                                                                          |
      +=======================+=======================+======================================================================================================================================================================+
      | project_id            | Yes                   | **Definition**                                                                                                                                                       |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | Project ID, which is used for resource isolation. For how to obtain a project ID, see :ref:`Obtaining a Project ID <dli_02_0183>`.                                   |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | Example: **48cc2c48765f481480c7db940d6409d1**                                                                                                                        |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | **Constraints**                                                                                                                                                      |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | None                                                                                                                                                                 |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | **Range**                                                                                                                                                            |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | The value can contain up to 64 characters. Only letters and digits are allowed.                                                                                      |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | **Default Value**                                                                                                                                                    |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | None                                                                                                                                                                 |
      +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | batch_id              | Yes                   | **Definition**                                                                                                                                                       |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | ID of a batch processing job. For details about how to obtain an ID, see **appId** in the response parameters in :ref:`Listing Batch Processing Jobs <dli_02_0125>`. |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | Example: **0a324461-d9d9-45da-a52a-3b3c7a3d809e**                                                                                                                    |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | **Constraints**                                                                                                                                                      |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | The parameter value must be a string matching the regular expression **^[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}$**.              |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | **Range**                                                                                                                                                            |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | None                                                                                                                                                                 |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | **Default Value**                                                                                                                                                    |
      |                       |                       |                                                                                                                                                                      |
      |                       |                       | None                                                                                                                                                                 |
      +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                              |
   +=================+=================+=================+==========================================================================================+
   | id              | No              | String          | **Definition**                                                                           |
   |                 |                 |                 |                                                                                          |
   |                 |                 |                 | ID of a batch processing job, which is in the universal unique identifier (UUID) format. |
   |                 |                 |                 |                                                                                          |
   |                 |                 |                 | **Range**                                                                                |
   |                 |                 |                 |                                                                                          |
   |                 |                 |                 | None                                                                                     |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------+
   | state           | No              | String          | **Definition**                                                                           |
   |                 |                 |                 |                                                                                          |
   |                 |                 |                 | Status of a batch processing job                                                         |
   |                 |                 |                 |                                                                                          |
   |                 |                 |                 | **Range**                                                                                |
   |                 |                 |                 |                                                                                          |
   |                 |                 |                 | -  **starting**: A job is being started.                                                 |
   |                 |                 |                 | -  **running**: A job is being executed.                                                 |
   |                 |                 |                 | -  **dead**: A session has exited.                                                       |
   |                 |                 |                 | -  **success**: A session is successfully stopped.                                       |
   |                 |                 |                 | -  **recovering**: A job is being restored.                                              |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {"id":"0a324461-d9d9-45da-a52a-3b3c7a3d809e","state":"Success"}

Status Codes
------------

:ref:`Table 3 <dli_02_0127__tb12870f1c5f24b27abd55ca24264af36>` describes status codes.

.. _dli_02_0127__tb12870f1c5f24b27abd55ca24264af36:

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
