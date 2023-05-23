:original_name: dli_02_0307.html

.. _dli_02_0307:

Creating a Queue
================

Scenario Description
--------------------

This section describes how to create and query a queue using APIs.

Constraints
-----------

-  Queues created using this API will be bound to specified compute resources.
-  It takes 6 to 10 minutes to start a job using a new queue for the first time.

Involved APIs
-------------

-  :ref:`Creating a Queue <dli_02_0194>`: Create a queue.
-  :ref:`Viewing Details of a Queue <dli_02_0016>`: Ensure that the queue is successfully created.

Procedure
---------

#. Create a queue.

   -  API

      URI format: POST /v1.0/{project_id}/queues

      -  Obtain the value of {project_id} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Queue <dli_02_0194>`.

   -  Request example

      -  Description: Create an SQL queue named **queue1** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/queues

      -  Body:

         .. code-block::

            {
                "queue_name": "queue1",
                "description": "test",
                "cu_count": 16,
                "resource_mode": 1,
                "queue_type": "sql"
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": "",
           "queue_name": "queue1"
         }

#. Verify that the queue is created successfully.

   -  API

      URI format: GET /v1.0/{*project_id*}/queues/{*queue_name*}

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the query parameters, see :ref:`Viewing Details of a Queue <dli_02_0016>`.

   -  Request example

      -  Description: Query details about queue1 in the project whose ID is 48cc2c48765f481480c7db940d6409d1.

      -  Example URL: GET https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/queues/queue1

      -  Body:

         .. code-block::

            {}

   -  Example response

      .. code-block::

         {
             "is_success": true,
             "message": "",
             "owner": "testuser",
             "description": "",
             "queue_name": "queue1",
             "create_time": 1587613028851,
             "queue_type": "sql",
             "cu_count": 16,
             "resource_id": "03d51b88-db63-4611-b779-9a72ba0cf58b",
             "resource_mode": 0
         }
