:original_name: dli_02_0316.html

.. _dli_02_0316:

Generating a Static Stream Graph for a Flink SQL Job
====================================================

Function
--------

This API is used to generate a static stream graph for a Flink SQL job.

URI
---

-  URI format

   POST /v3/{*project_id*}/streaming/jobs/{*job_id*}/gen-graph

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

   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | Parameter               | Mandatory       | Type            | Description                                                                           |
   +=========================+=================+=================+=======================================================================================+
   | sql_body                | Yes             | String          | SQL                                                                                   |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | cu_number               | No              | Integer         | Total number of CUs.                                                                  |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | manager_cu_number       | No              | Integer         | Number of CUs of the management unit.                                                 |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | parallel_number         | No              | Integer         | Maximum degree of parallelism.                                                        |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | tm_cus                  | No              | Integer         | Number of CUs in a taskManager.                                                       |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | tm_slot_num             | No              | Integer         | Number of slots in a taskManager.                                                     |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | operator_config         | No              | String          | Operator configurations.                                                              |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | static_estimator        | No              | Boolean         | Whether to estimate static resources.                                                 |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | job_type                | No              | String          | Job types. Only **flink_opensource_sql_job job** is supported.                        |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | graph_type              | No              | String          | Stream graph type. Currently, the following two types of stream graphs are supported: |
   |                         |                 |                 |                                                                                       |
   |                         |                 |                 | -  **simple_graph**: Simplified stream graph                                          |
   |                         |                 |                 | -  **job_graph**: Static stream graph                                                 |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | static_estimator_config | No              | String          | Traffic or hit ratio of each operator, which is a character string in JSON format.    |
   +-------------------------+-----------------+-----------------+---------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter    | Mandatory | Type    | Description                                                                                                                 |
   +==============+===========+=========+=============================================================================================================================+
   | is_success   | Yes       | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message      | Yes       | String  | System prompt. If execution succeeds, the message may be left blank.                                                        |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | error_code   | Yes       | String  | Error codes.                                                                                                                |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | stream_graph | Yes       | String  | Description of a static stream graph.                                                                                       |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

.. code-block::

   {
      "cu_number": 4,
      "manager_cu_number": 1,
      "parallel_number": 4,
      "tm_cus": 1,
      "tm_slot_num": 1,
      "sql_body": "",
      "operator_config": "",
      "static_estimator": true,
      "job_type": "flink_opensource_sql_job",
      "graph_type": "job_graph"
    }

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "",
       "error_code": "",
       "stream_graph": "{\n  \"nodes\" : [ {\n    \"id\" : 1,\n    \"operator_id\" : \"bc764cd8ddf7a0cff126f51c16239658\",\n    \"type\" : \"Source\",\n
    \"contents\" : \"kafkaSource\",\n    \"parallelism\" : 1\n  }, {\n    \"id\" : 2,\n    \"operator_id\" : \"0a448493b4782967b150582570326227\",\n    \"type\" : \"select\",\n    \"contents\" : \"car_id, car_owner, car_brand, car_speed\",\n    \"parallelism\" : 1,\n    \"predecessors\" : [ {\n      \"id\" : 1\n    } ]\n  }, {\n    \"id\" : 4,\n    \"operator_id\" : \"6d2677a0ecc3fd8df0b72ec675edf8f4\",\n    \"type\" : \"Sink\",\n    \"contents\" : \"kafkaSink\",\n    \"parallelism\" : 1,\n    \"predecessors\" : [ {\n      \"id\" : 2\n    } ]\n  } ]\n}"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0316__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0316__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ===============================
   Status Code Description
   =========== ===============================
   200         The operation is successful.
   400         The input parameter is invalid.
   =========== ===============================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
