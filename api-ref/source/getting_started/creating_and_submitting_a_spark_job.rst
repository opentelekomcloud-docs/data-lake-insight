:original_name: dli_02_0309.html

.. _dli_02_0309:

Creating and Submitting a Spark Job
===================================

Scenario
--------

This section describes how to create and submit Spark jobs using APIs.

Notes and Constraints
---------------------

-  It takes 6 to 10 minutes to start a job using a new queue for the first time.

Involved APIs
-------------

-  :ref:`Creating an Elastic Resource Pool <dli_02_0326>`: Create an elastic resource pool.
-  :ref:`Creating a Queue <dli_02_0194>`: Create queues within the elastic resource pool.
-  :ref:`Uploading a Package Group (Deprecated) <dli_02_0130>`: Upload the resource package required by the Spark job.
-  :ref:`Querying Resource Packages in a Group (Deprecated) <dli_02_0172>`: Check whether the uploaded resource package is correct.
-  :ref:`Creating a Batch Processing Job <dli_02_0124>`: Create and submit a Spark batch processing job.
-  :ref:`Querying a Batch Job Status <dli_02_0127>`: View the status of a batch processing job.
-  :ref:`Querying Batch Job Logs (Deprecated) <dli_02_0128>`: View batch processing job logs.

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

#. .. _dli_02_0309__li117291344122510:

   Upload a package group.

   -  API

      URI format: POST /v2.0/{*project_id*}/resources

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Uploading a Package Group (Deprecated) <dli_02_0130>`.

   -  Example request

      -  Description: Upload resources in the GATK group to the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/resources

      -  Body:

         .. code-block::

            {
                "paths": [
                    "https://test.obs.xxx.com/txr_test/jars/spark-sdv-app.jar"
                ],
                "kind": "jar",
                "group": "gatk",
                "is_async":"true"
            }

   -  Example response

      .. code-block::

         {
             "group_name": "gatk",
             "status": "READY",
             "resources": [
                 "spark-sdv-app.jar",
                 "wordcount",
                 "wordcount.py"
             ],
             "details": [
                 {
                     "create_time": 0,
                     "update_time": 0,
                     "resource_type": "jar",
                     "resource_name": "spark-sdv-app.jar",
                     "status": "READY",
                     "underlying_name": "987e208d-d46e-4475-a8c0-a62f0275750b_spark-sdv-app.jar"
                 },
                 {
                     "create_time": 0,
                     "update_time": 0,
                     "resource_type": "jar",
                     "resource_name": "wordcount",
                     "status": "READY",
                     "underlying_name": "987e208d-d46e-4475-a8c0-a62f0275750b_wordcount"
                 },
                 {
                     "create_time": 0,
                     "update_time": 0,
                     "resource_type": "jar",
                     "resource_name": "wordcount.py",
                     "status": "READY",
                     "underlying_name": "987e208d-d46e-4475-a8c0-a62f0275750b_wordcount.py"
                 }
             ],
             "create_time": 1551334579654,
             "update_time": 1551345369070
         }

#. .. _dli_02_0309__li970315312304:

   View resource packages in a group.

   -  API

      URI format: GET /v2.0/{*project_id*}/resources/{*resource_name*}

      -  Obtain the value of {project_id} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the query parameters, see :ref:`Creating a Table (Deprecated) <dli_02_0034>`.

   -  Example request

      -  Description: Query the resource package named **luxor-router-1.1.1.jar** in the GATK group under the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: GET https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/resources/luxor-router-1.1.1.jar?group=gatk

      -  Body:

         .. code-block::

            {}

   -  Example response

      .. code-block::

         {
             "create_time": 1522055409139,
             "update_time": 1522228350501,
             "resource_type": "jar",
             "resource_name": "luxor-router-1.1.1.jar",
             "status": "uploading",
             "underlying_name": "7885d26e-c532-40f3-a755-c82c442f19b8_luxor-router-1.1.1.jar",
             "owner": "****"
         }

#. Create and submit a Spark batch processing job.

   -  API

      URI format: POST /v2.0/{*project_id*}/batches

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Batch Processing Job <dli_02_0124>`.

   -  Example request

      -  Description: In the **48cc2c48765f481480c7db940d6409d1** project, create a batch processing job named **TestDemo4** in **queue1**.

      -  Example URL: POST https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/batches

      -  Body:

         .. code-block::

            {
              "sc_type": "A",
              "jars": [

            "spark-examples_2.11-2.1.0.luxor.jar"
              ],
              "driverMemory": "1G",
              "driverCores": 1,
              "executorMemory": "1G",
              "executorCores": 1,
              "numExecutors": 1,
              "queue": "queuel",
              "file":
            "spark-examples_2.11-2.1.0.luxor.jar",
              "className":
            "org.apache.spark.examples.SparkPi",
              "minRecoveryDelayTime": 10000,
              "maxRetryTimes": 20
            }

   -  Example response

      .. code-block::

         {
           "id": "07a3e4e6-9a28-4e92-8d3f-9c538621a166",
           "appId": "",
           "name": "",
           "owner": "test1",
           "proxyUser": "",
           "state": "starting",
           "kind": "",
           "log": [],
           "sc_type": "CUSTOMIZED",
           "cluster_name": "aaa",
           "queue": "queue1",
           "create_time": 1607589874156,
           "update_time": 1607589874156
         }

#. Query a batch job status.

   -  API

      URI format: GET /v2.0/{*project_id*}/batches/{*batch_id*}/state

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the query parameters, see :ref:`Querying a Batch Job Status <dli_02_0127>`.

   -  Example request

      -  Description: Query the status of the batch processing job whose ID is **0a324461-d9d9-45da-a52a-3b3c7a3d809e** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: GET https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/batches/0a324461-d9d9-45da-a52a-3b3c7a3d809e/state

      -  Body:

         .. code-block::

            {}

   -  Example response

      .. code-block::

         {
            "id":"0a324461-d9d9-45da-a52a-3b3c7a3d809e",
            "state":"Success"
         }

#. Query batch job logs.

   -  API

      URI format: GET /v2.0/{*project_id*}/batches/{*batch_id*}/log

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the query parameters, see :ref:`Querying Batch Job Logs (Deprecated) <dli_02_0128>`.

   -  Example request

      -  Description: Query the background logs of the batch processing job **0a324461-d9d9-45da-a52a-3b3c7a3d809e** in the **48cc2c48765f481480c7db940d6409d1** project.

      -  Example URL: GET https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/batches/0a324461-d9d9-45da-a52a-3b3c7a3d809e/log

      -  Body:

         .. code-block::

            {}

   -  Example response

      .. code-block::

         {
             "id": "0a324461-d9d9-45da-a52a-3b3c7a3d809e",
             "from": 0,
             "total": 3,
             "log": [
                    "Detailed information about job logs"
             ]
         }
