:original_name: dli_02_0316.html

.. _dli_02_0316:

Generating a Static Stream Graph for a Flink SQL Job
====================================================

Function
--------

This API is used to generate a static stream graph for a Flink SQL job.

Flink 1.15 does not support the generation of static stream graphs.

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

   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                          |
   +=========================+=================+=================+======================================================================================================================================================================================================================================================================================================+
   | sql_body                | Yes             | String          | SQL                                                                                                                                                                                                                                                                                                  |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cu_number               | No              | Integer         | Total number of CUs used by the job configured on the job editing page, which should match the actual number of CUs used. The actual number of CUs used is determined by the number of parallel operators.                                                                                           |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | Total number of CUs used by the job = Number of manager CUs + (Total number of concurrent operators / Number of slots of a TaskManager) x Number of TaskManager CUs                                                                                                                                  |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | manager_cu_number       | No              | Integer         | Number of CUs of the management unit.                                                                                                                                                                                                                                                                |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parallel_number         | No              | Integer         | Maximum degree of parallelism.                                                                                                                                                                                                                                                                       |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | Concurrent tasks of each job operator. Appropriately increasing the value will improve the overall computing performance of a job. Considering switchover overheads due to increasing threads, the maximum value is four times the number of CUs. One to two times the number of CUs is the optimal. |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_cus                  | No              | Integer         | Number of CUs in a taskManager.                                                                                                                                                                                                                                                                      |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_slot_num             | No              | Integer         | Number of slots in a taskManager.                                                                                                                                                                                                                                                                    |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | operator_config         | No              | String          | Operator configurations.                                                                                                                                                                                                                                                                             |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | You can call this API to obtain the operator ID. That is, the ID in **operator_list** contained in **stream_graph** in the response message is the operator ID.                                                                                                                                      |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | static_estimator        | No              | Boolean         | Whether to estimate static resources.                                                                                                                                                                                                                                                                |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | If this parameter is set to **true**, resources used by the job are estimated based on the operator ID and traffic.                                                                                                                                                                                  |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | static_estimator_config | No              | String          | Traffic or hit ratio of each operator, which is a string in JSON format.                                                                                                                                                                                                                             |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | This parameter is mandatory when **static_estimator** is set to **true**. During the configuration, the operator ID and operator traffic configuration are required.                                                                                                                                 |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | -  You can call this API to obtain the operator ID. That is, the ID in **operator_list** contained in **stream_graph** in the response message is the operator ID.                                                                                                                                   |
   |                         |                 |                 | -  The operator traffic is estimated based on the actual service conditions.                                                                                                                                                                                                                         |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_type                | No              | String          | Job types.                                                                                                                                                                                                                                                                                           |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | Only **flink_opensource_sql_job job** is supported.                                                                                                                                                                                                                                                  |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | graph_type              | No              | String          | Stream graph type. Currently, the following two types of stream graphs are supported:                                                                                                                                                                                                                |
   |                         |                 |                 |                                                                                                                                                                                                                                                                                                      |
   |                         |                 |                 | -  **simple_graph**: Simplified stream graph                                                                                                                                                                                                                                                         |
   |                         |                 |                 | -  **job_graph**: Static stream graph                                                                                                                                                                                                                                                                |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink_version           | No              | String          | Flink version. Currently, only 1.10 and 1.12 are supported.                                                                                                                                                                                                                                          |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

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

Generate a static stream graph for a Flink SQL job.

.. code-block::

   {
     "job_type": "flink_opensource_sql_job",
     "graph_type": "job_graph",
     "sql_body": "create table orders(\r\n  name string,\r\n  num int\r\n) with (\r\n  'connector' = 'datagen',\r\n  'rows-per-second' = '1', --Generates a data record per second.\r\n  'fields.name.kind' = 'random', --Specify a random generator for the user_id field.\r\n  'fields.name.length' = '5' --Limit the length of user_id to 3.\r\n);\r\n \r\nCREATE TABLE sink_table (\r\n  name string,\r\n  num int\r\n) WITH (\r\n   'connector' = 'print'\r\n);\r\nINSERT into sink_table SELECT * FROM orders;",
     "cu_number": 2,
     "manager_cu_number": 1,
     "parallel_number": 2,
     "tm_cus": 1,
     "tm_slot_num": 0,
     "operator_config": "",
     "static_estimator": true,
     "flink_version": "1.12",
     "static_estimator_config": "{\"operator_list\":[{\"id\":\"0a448493b4782967b150582570326227\",\"output_rate\":1000},{\"id\":\"bc764cd8ddf7a0cff126f51c16239658\",\"output_rate\":1000}]}"
   }

Example Response
----------------

.. code-block::

   {
       "message": "",
       "is_success": true,
       "error_code": "",
       "stream_graph": "{\n  \"jid\" : \"44334c4259f6714bddef1ac525364052\",\n  \"name\" : \"InternalJob_1715392878428\",\n  \"nodes\" : [ {\n    \"id\" : \"0a448493b4782967b150582570326227\",\n    \"parallelism\" : 1,\n    \"operator\" : \"\",\n    \"operator_strategy\" : \"\",\n    \"description\" : \"Sink: Sink(table=[default_catalog.default_database.sink_table], fields=[name, num])\",\n    \"chain_operators_id\" : [ \"0a448493b4782967b150582570326227\" ],\n    \"inputs\" : [ {\n      \"num\" : 0,\n      \"id\" : \"bc764cd8ddf7a0cff126f51c16239658\",\n      \"ship_strategy\" : \"FORWARD\",\n      \"exchange\" : \"pipelined_bounded\"\n    } ],\n    \"optimizer_properties\" : {}\n  }, {\n    \"id\" : \"bc764cd8ddf7a0cff126f51c16239658\",\n    \"parallelism\" : 2,\n    \"operator\" : \"\",\n    \"operator_strategy\" : \"\",\n    \"description\" : \"Source: TableSourceScan(table=[[default_catalog, default_database, orders]], fields=[name, num])\",\n    \"chain_operators_id\" : [ \"bc764cd8ddf7a0cff126f51c16239658\" ],\n    \"optimizer_properties\" : {}\n  } ],\n  \"operator_list\" : [ {\n    \"id\" : \"0a448493b4782967b150582570326227\",\n    \"name\" : \"Sink: Sink(table=[default_catalog.default_database.sink_table], fields=[name, num])\",\n    \"type\" : \"Sink\",\n    \"contents\" : \"Sink(table=[default_catalog.default_database.sink_table], fields=[name, num])\",\n    \"parallelism\" : 1,\n    \"tags\" : \"[SINK]\",\n    \"input_operators_id\" : [ \"bc764cd8ddf7a0cff126f51c16239658\" ]\n  }, {\n    \"id\" : \"bc764cd8ddf7a0cff126f51c16239658\",\n    \"name\" : \"Source: TableSourceScan(table=[[default_catalog, default_database, orders]], fields=[name, num])\",\n    \"type\" : \"Source\",\n    \"contents\" : \"TableSourceScan(table=[[default_catalog, default_database, orders]], fields=[name, num])\",\n    \"parallelism\" : 2,\n    \"tags\" : \"[PROCESS, UDF]\",\n    \"input_operators_id\" : [ ]\n  } ]\n}"
   }

To make it easier to view the response information, we format **stream_graph** as follows:

.. code-block::

       "jid": "65b6a7b0c1ad95b1722a92b49d2f6eba",
       "name": "InternalJob_1715392245413",
       "nodes": [
           {
               "id": "0a448493b4782967b150582570326227",
               "parallelism": 1,
               "operator": "",
               "operator_strategy": "",
               "description": "Sink: Sink(table=[default_catalog.default_database.sink_table], fields=[name, num])",
               "chain_operators_id": [
                   "0a448493b4782967b150582570326227"
               ],
               "inputs": [
                   {
                       "num": 0,
                       "id": "bc764cd8ddf7a0cff126f51c16239658",
                       "ship_strategy": "FORWARD",
                       "exchange": "pipelined_bounded"
                   }
               ],
               "optimizer_properties": {

               }
           },
           {
               "id": "bc764cd8ddf7a0cff126f51c16239658",
               "parallelism": 2,
               "operator": "",
               "operator_strategy": "",
               "description": "Source: TableSourceScan(table=[[default_catalog, default_database, orders]], fields=[name, num])",
               "chain_operators_id": [
                   "bc764cd8ddf7a0cff126f51c16239658"
               ],
               "optimizer_properties": {

               }
           }
       ],
       "operator_list": [
           {
               "id": "0a448493b4782967b150582570326227",
               "name": "Sink: Sink(table=[default_catalog.default_database.sink_table], fields=[name, num])",
               "type": "Sink",
               "contents": "Sink(table=[default_catalog.default_database.sink_table], fields=[name, num])",
               "parallelism": 1,
               "tags": "[SINK]",
               "input_operators_id": [
                   "bc764cd8ddf7a0cff126f51c16239658"
               ]
           },
           {
               "id": "bc764cd8ddf7a0cff126f51c16239658",
               "name": "Source: TableSourceScan(table=[[default_catalog, default_database, orders]], fields=[name, num])",
               "type": "Source",
               "contents": "TableSourceScan(table=[[default_catalog, default_database, orders]], fields=[name, num])",
               "parallelism": 2,
               "tags": "[PROCESS, UDF]",
               "input_operators_id": [

               ]
           }
       ]
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

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
