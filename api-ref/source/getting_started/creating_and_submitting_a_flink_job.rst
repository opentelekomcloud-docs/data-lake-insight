:original_name: dli_02_0310.html

.. _dli_02_0310:

Creating and Submitting a Flink Job
===================================

Scenario
--------

This section describes how to create and run a user-defined Flink job using APIs.

Notes and Constraints
---------------------

-  It takes 6 to 10 minutes to start a job using a new queue for the first time.

Involved APIs
-------------

-  :ref:`Creating an Elastic Resource Pool <dli_02_0326>`: Create an elastic resource pool.
-  :ref:`Creating a Queue <dli_02_0194>`: Create queues within the elastic resource pool.
-  :ref:`Uploading a Package Group (Deprecated) <dli_02_0130>`: Upload the resource package required by the Flink custom job.
-  :ref:`Querying Resource Packages in a Group (Deprecated) <dli_02_0172>`: Check whether the uploaded resource package is correct.
-  :ref:`Creating a Flink Jar job <dli_02_0230>` Create a user-defined Flink job.
-  :ref:`Running Jobs in Batches <dli_02_0233>`: Run a user-defined Flink job.

Procedure
---------

#. Create an elastic resource pool named **elastic_pool_dli**.

   -  API

      URI format: POST /v3/{project_id}/elastic-resource-pools

      -  Obtain the value of *{project_id}* by referring to :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating an Elastic Resource Pool <dli_02_0326>`.

   -  Example request

      -  Description: Create an elastic resource pool named **elastic_pool_dli** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{endpoint}/v3/48cc2c48765f481480c7db940d6409d1/elastic-resource-pools

      -  Body:

         .. code-block::

            {
              "elastic_resource_pool_name" : "elastic_pool_dli",
              "description" : "test",
              "cidr_in_vpc" : "172.16.0.0/14",
              "charging_mode" : "1",
              "max_cu" : 64,
              "min_cu" : 64
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }

#. Create a queue named **queue1** in the elastic resource pool.

   -  API

      URI format: POST /v1.0/{project_id}/queues

      -  Obtain the value of *{project_id}* by referring to :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Queue <dli_02_0194>`.

   -  Example request

      -  Description: Create an elastic resource pool named **queue1** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/queues

      -  Body:

         .. code-block::

            {
                "queue_name": "queue1",
                "queue_type": "sql",
                "description": "test",
                "cu_count": 16,
                "enterprise_project_id": "elastic_pool_dli"
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }

#. Upload the resource package of the user-defined Flink job. For details, see :ref:`3 <dli_02_0309__li117291344122510>`.
#. Query resource packages in a group. For details, see :ref:`4 <dli_02_0309__li970315312304>`.
#. Create a custom flink job.

   -  API

      URI format: POST /v1.0/{*project_id*}/streaming/flink-jobs

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Database (Deprecated) <dli_02_0028>`.

   -  Example request

      -  Description: Create a user-defined Flink job in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/streaming/flink-jobs

      -  Body:

         .. code-block::

            {
                "name": "test",
                "desc": "job for test",
                "queue_name": "queue1",
                "manager_cu_number": 1,
                "cu_number": 2,
                "parallel_number": 1,
                "tm_cus": 1,
                "tm_slot_num": 1,
                "log_enabled": true,
                "obs_bucket": "bucketName",
                "smn_topic": "topic",
                "main_class": "org.apache.flink.examples.streaming.JavaQueueStream",
                "restart_when_exception": false,
                "entrypoint": "javaQueueStream.jar",
                "entrypoint_args":"-windowSize 2000 -rate3",
                "dependency_jars": [
                    "myGroup/test.jar",
                    "myGroup/test1.jar"
                ],
                "dependency_files": [
                    "myGroup/test.csv",
                    "myGroup/test1.csv"
                ]
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": "A Flink job is created successfully.",
           "job": {
             "job_id": 138,
             "status_name": "job_init",
             "status_desc": ""
           }
         }

#. Run jobs in batches.

   -  API

      URI format: POST /v1.0/{*project_id*}/streaming/jobs/run

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Running Jobs in Batches <dli_02_0233>`.

   -  Example request

      -  Description: Run the jobs whose **job_id** is **298765** and **298766** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/streaming/jobs/run

      -  Body:

         .. code-block::

            {
                "job_ids": [131,130,138,137],
                "resume_savepoint": true
            }

   -  Example response

      .. code-block::

         [
             {
                 "is_success": "true",
                 "message": "The request for submitting DLI jobs is delivered successfully."
             },
             {
                 "is_success": "true",
                 "message": "The request for submitting DLI jobs is delivered successfully."
             },
             {
                 "is_success": "true",
                 "message": "The request for submitting DLI jobs is delivered successfully."
             },
             {
                 "is_success": "true",
                 "message": "The request for submitting DLI jobs is delivered successfully."
             }
         ]
