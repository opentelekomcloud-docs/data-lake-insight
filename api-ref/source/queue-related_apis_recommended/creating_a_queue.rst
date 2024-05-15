:original_name: dli_02_0194.html

.. _dli_02_0194:

Creating a Queue
================

Function
--------

This API is used to create a queue. The queue will be bound to specified compute resources.

.. note::

   It takes 5 to 15 minutes to start a job using a new queue for the first time.

URI
---

-  URI format

   POST /v1.0/{project_id}/queues

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory       | Type             | Description                                                                                                                                                                                                                                           |
   +=======================+=================+==================+=======================================================================================================================================================================================================================================================+
   | queue_name            | Yes             | String           | Name of a newly created resource queue. The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). Length range: 1 to 128 characters.                                            |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | .. note::                                                                                                                                                                                                                                             |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  |    The queue name is case-insensitive. The uppercase letters will be automatically converted to lowercase letters.                                                                                                                                    |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_type            | No              | String           | Queue type. The options are as follows:                                                                                                                                                                                                               |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | -  **sql**: Queues used to run SQL jobs                                                                                                                                                                                                               |
   |                       |                 |                  | -  **general**: Queues used to run Flink and Spark Jar jobs.                                                                                                                                                                                          |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | .. note::                                                                                                                                                                                                                                             |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  |    If the type is not specified, the default value **sql** is used.                                                                                                                                                                                   |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description           | No              | String           | Description of a queue.                                                                                                                                                                                                                               |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cu_count              | Yes             | Integer          | Minimum number of CUs that are bound to a queue. Currently, the value can only be **16**, **64**, or **256**.                                                                                                                                         |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enterprise_project_id | No              | String           | Enterprise project ID. The value **0** indicates the default enterprise project.                                                                                                                                                                      |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | .. note::                                                                                                                                                                                                                                             |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  |    Users who have enabled Enterprise Management can set this parameter to bind a specified project.                                                                                                                                                   |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | platform              | No              | String           | CPU architecture of compute resources.                                                                                                                                                                                                                |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | -  x86_64                                                                                                                                                                                                                                             |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_mode         | No              | Integer          | Queue resource mode. The options are as follows:                                                                                                                                                                                                      |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | **0**: shared resource mode                                                                                                                                                                                                                           |
   |                       |                 |                  |                                                                                                                                                                                                                                                       |
   |                       |                 |                  | **1**: dedicated resource mode                                                                                                                                                                                                                        |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | labels                | No              | Array of Strings | Tag information of the queue to be created. Currently, the tag information includes whether the queue is cross-AZ (JSON string). The value can only be **2**, that is, a dual-AZ queue whose compute resources are distributed in two AZs is created. |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags                  | No              | Array of objects | Queue tags for identifying cloud resources. A tag consists of a key and tag value. For details, see :ref:`Table 3 <dli_02_0194__table9391124139>`.                                                                                                    |
   +-----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0194__table9391124139:

.. table:: **Table 3** tags parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                     |
   +=================+=================+=================+=================================================================================================================================================================================================================+
   | Key             | Yes             | String          | Tag key.                                                                                                                                                                                                        |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 | .. note::                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 |    A tag key can contain a maximum of 128 characters. Only letters, digits, spaces, and special characters ``(_.:=+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Tag value.                                                                                                                                                                                                      |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 | .. note::                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 |    A tag value can contain a maximum of 255 characters. Only letters, digits, spaces, and special characters ``(_.:=+-@)`` are allowed. The value cannot start or end with a space.                             |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 4** Response parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                 |
   +=================+=================+=================+=============================================================================================================================+
   | is_success      | No              | Boolean         | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message         | No              | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
   | queue_name      | No              | String          | Name of the created queue.                                                                                                  |
   |                 |                 |                 |                                                                                                                             |
   |                 |                 |                 | .. note::                                                                                                                   |
   |                 |                 |                 |                                                                                                                             |
   |                 |                 |                 |    The queue name is case-insensitive. The uppercase letters will be automatically converted to lowercase letters.          |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Create a dedicated general-purpose queue named **queue1**, with specifications of 16 CUs and compute resources distributed in two AZs.

.. code-block::

   {
       "queue_name": "queue1",
       "description": "test",
       "cu_count": 16,
       "resource_mode": 1,
       "queue_type": "general",
       "labels": ["multi_az=2"]
   }

Creating a queue in a specified elastic resource pool

.. code-block::

   {
       "queue_name": "queue2",
       "description": "test_esp",
       "cu_count": 16,
       "resource_mode": 1,
       "enterprise_project_id": "0",
       "queue_type": "general",
       "labels": ["multi_az=2"],
       "elastic_resource_pool_name": "elastic_pool_0622_10"
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "queue_name": "queue1"
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0194__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0194__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

   =========== ================================
   Status Code Description
   =========== ================================
   200         The job is created successfully.
   400         Request error.
   500         Internal service error.
   =========== ================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
