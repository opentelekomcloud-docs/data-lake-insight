:original_name: dli_02_0016.html

.. _dli_02_0016:

Viewing Details of a Queue
==========================

Function
--------

This API is used to list details of a specific queue in a project.

URI
---

-  URI format

   GET /v1.0/{project_id}/queues/{queue_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                   |
      +=================+=================+=================+===============================================================================================================================================+
      | project_id      | Yes             | String          | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name      | Yes             | String          | Specifies the name of a queue to be queried.                                                                                                  |
      |                 |                 |                 |                                                                                                                                               |
      |                 |                 |                 | .. note::                                                                                                                                     |
      |                 |                 |                 |                                                                                                                                               |
      |                 |                 |                 |    The queue name is case-insensitive. The uppercase letters will be automatically converted to lowercase letters.                            |
      +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response parameters

   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory       | Type            | Description                                                                                                                 |
   +============================+=================+=================+=============================================================================================================================+
   | is_success                 | No              | Boolean         | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message                    | No              | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | queueName                  | No              | String          | Name of a queue.                                                                                                            |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 | .. note::                                                                                                                   |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 |    The queue name is case-insensitive. The uppercase letters will be automatically converted to lowercase letters.          |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | description                | No              | String          | Queue description.                                                                                                          |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | owner                      | No              | String          | User who creates a queue.                                                                                                   |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | create_time                | No              | Long            | Time when the queue is created. The timestamp is expressed in milliseconds.                                                 |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | queueType                  | No              | String          | Indicates the queue type.                                                                                                   |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 | -  sql                                                                                                                      |
   |                            |                 |                 | -  general                                                                                                                  |
   |                            |                 |                 | -  all                                                                                                                      |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 | If this parameter is not specified, the default value **sql** is used.                                                      |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | cuCount                    | No              | Integer         | Number of compute units (CUs) bound to a queue, that is, the number of CUs in the current queue.                            |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | resource_id                | No              | String          | Resource ID of a queue.                                                                                                     |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | resource_mode              | No              | Integer         | Resource mode                                                                                                               |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 | -  **0**: Shared queue                                                                                                      |
   |                            |                 |                 | -  **1**: Dedicated queue                                                                                                   |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | enterprise_project_id      | No              | String          | Enterprise project ID.                                                                                                      |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 | **0** indicates the default enterprise project.                                                                             |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 | .. note::                                                                                                                   |
   |                            |                 |                 |                                                                                                                             |
   |                            |                 |                 |    Users who have enabled Enterprise Management can set this parameter to bind a specified project.                         |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | cu_spec                    | No              | Integer         | Specifications of a queue.                                                                                                  |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | cu_scale_out_limit         | No              | Integer         | Upper limit of the CU value for elastic scaling of the current queue.                                                       |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | cu_scale_in_limit          | No              | Integer         | Lower limit of the CU value for elastic scaling of the current queue.                                                       |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | elastic_resource_pool_name | No              | String          | Name of the elastic resource pool.                                                                                          |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "",
       "owner": "testuser",
       "description": "",
       "queueName": "test",
       "create_time": 1587613028851,
       "queueType": "general",
       "cuCount": 16,
       "resource_id": "03d51b88-db63-4611-b779-9a72ba0cf58b",
       "resource_mode": 0
   ,
       "resource_type": "vm",
        "cu_spec": 16
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0016__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0016__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 3** Status codes

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
