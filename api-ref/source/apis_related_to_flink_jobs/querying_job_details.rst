:original_name: dli_02_0235.html

.. _dli_02_0235:

Querying Job Details
====================

Function
--------

This API is used to query details of a job.

URI
---

-  URI format

   GET /v1.0/{project_id}/streaming/jobs/{job_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | String | Job ID.                                                                                                                                       |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | job_detail | No        | Object  | Job details. For details, see :ref:`Table 3 <dli_02_0235__table15129182752011>`.                                            |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0235__table15129182752011:

.. table:: **Table 3** job_detail parameters

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                        |
   +=================+=================+=================+====================================================================================================+
   | job_id          | No              | Long            | Job ID.                                                                                            |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | name            | No              | String          | Name of the job. Length range: 0 to 57 characters.                                                 |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | desc            | No              | String          | Job description. Length range: 0 to 512 characters.                                                |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | job_type        | No              | String          | Job type.                                                                                          |
   |                 |                 |                 |                                                                                                    |
   |                 |                 |                 | -  **flink_sql_job**: Flink SQL job                                                                |
   |                 |                 |                 | -  **flink_jar_job**: User-defined Flink job                                                       |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | status          | No              | String          | Job status.                                                                                        |
   |                 |                 |                 |                                                                                                    |
   |                 |                 |                 | Available job statuses are as follows:                                                             |
   |                 |                 |                 |                                                                                                    |
   |                 |                 |                 | -  **job_init**: The job is in the draft status.                                                   |
   |                 |                 |                 | -  **job_submitting**: The job is being submitted.                                                 |
   |                 |                 |                 | -  **job_submit_fail**: The job fails to be submitted.                                             |
   |                 |                 |                 | -  **job_running**: The job is running. (After the job is submitted, a normal result is returned.) |
   |                 |                 |                 | -  **job_running_exception** (The job stops running due to an exception.)                          |
   |                 |                 |                 | -  **job_downloading**: The job is being downloaded.                                               |
   |                 |                 |                 | -  **job_idle**: The job is idle.                                                                  |
   |                 |                 |                 | -  **job_canceling**: The job is being stopped.                                                    |
   |                 |                 |                 | -  **job_cancel_success**: The job has been stopped.                                               |
   |                 |                 |                 | -  **job_cancel_fail**: The job fails to be stopped.                                               |
   |                 |                 |                 | -  **job_savepointing**: The savepoint is being created.                                           |
   |                 |                 |                 | -  **job_finish**: The job is completed.                                                           |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | status_desc     | No              | String          | Description of job status.                                                                         |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | create_time     | No              | Long            | Time when a job is created.                                                                        |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | start_time      | No              | Long            | Time when a job is started.                                                                        |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | user_id         | No              | String          | ID of the user who creates the job.                                                                |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | queue_name      | No              | String          | Name of a queue. Length range: 1 to 128 characters.                                                |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | project_id      | No              | String          | ID of the project to which a job belongs.                                                          |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | sql_body        | No              | String          | Stream SQL statement.                                                                              |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | savepoint_path  | No              | String          | Path for storing manually generated checkpoints.                                                   |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | run_mode        | No              | String          | Job running mode. The options are as follows:                                                      |
   |                 |                 |                 |                                                                                                    |
   |                 |                 |                 | -  **shared_cluster**: indicates that the job is running on a shared cluster.                      |
   |                 |                 |                 | -  **exclusive_cluster**: indicates that the job is running on an exclusive cluster.               |
   |                 |                 |                 | -  **edge_node**: indicates that the job is running on an edge node.                               |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | job_config      | No              | Object          | Job configurations. Refer to :ref:`Table 4 <dli_02_0235__table10265738183119>` for details.        |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | main_class      | No              | String          | Main class of a JAR package, for example, **org.apache.spark.examples.streaming.JavaQueueStream**. |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | entrypoint_args | No              | String          | Running parameter of a JAR package job. Multiple parameters are separated by spaces.               |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | execution_graph | No              | String          | Job execution plan.                                                                                |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | update_time     | No              | Long            | Time when a job is updated.                                                                        |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+

.. _dli_02_0235__table10265738183119:

.. table:: **Table 4** **job_config** parameters

   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory       | Type             | Description                                                                                                                                                             |
   +=========================+=================+==================+=========================================================================================================================================================================+
   | checkpoint_enabled      | No              | Boolean          | Whether to enable the automatic job snapshot function.                                                                                                                  |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | -  **true**: The automatic job snapshot function is enabled.                                                                                                            |
   |                         |                 |                  | -  **false**: The automatic job snapshot function is disabled.                                                                                                          |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | The default value is **false**.                                                                                                                                         |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_interval     | No              | Integer          | Snapshot interval. The unit is second. The default value is **10**.                                                                                                     |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_mode         | No              | String           | Snapshot mode. There are two options:                                                                                                                                   |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | -  **exactly_once**: indicates that data is processed only once.                                                                                                        |
   |                         |                 |                  | -  **at_least_once**: indicates that data is processed at least once.                                                                                                   |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | The default value is **exactly_once**.                                                                                                                                  |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | log_enabled             | No              | Boolean          | Whether to enable the log storage function. The default value is **false**.                                                                                             |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_bucket              | No              | String           | Name of an OBS bucket.                                                                                                                                                  |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | root_id                 | No              | Integer          | Parent job ID.                                                                                                                                                          |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | edge_group_ids          | No              | Array of Strings | List of edge computing group IDs. Use commas (,) to separate multiple IDs.                                                                                              |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | manager_cu_number       | No              | Integer          | Number of CUs of the management unit. The default value is **1**.                                                                                                       |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | graph_editor_enabled    | No              | Boolean          | Whether to enable flow diagram editing. The default value is **false**.                                                                                                 |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | graph_editor_data       | No              | String           | Data of flow diagram editing. The default value is **null**.                                                                                                            |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | executor_number         | No              | Integer          | Number of compute nodes in a job.                                                                                                                                       |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | executor_cu_number      | No              | Integer          | Number of CUs in a compute node.                                                                                                                                        |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cu_number               | No              | Integer          | Number of CUs selected for a job. This parameter is valid only when **show_detail** is set to **true**.                                                                 |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | -  Minimum value: **2**                                                                                                                                                 |
   |                         |                 |                  | -  Maximum value: **400**                                                                                                                                               |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | The default value is **2**.                                                                                                                                             |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parallel_number         | No              | Integer          | Number of concurrent jobs set by a user. This parameter is valid only when **show_detail** is set to **true**.                                                          |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | -  Minimum value: **1**                                                                                                                                                 |
   |                         |                 |                  | -  Maximum value: **2000**                                                                                                                                              |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | The default value is **1**.                                                                                                                                             |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | smn_topic               | No              | String           | SMN topic name. If a job fails, the system will send a message to users subscribed to this SMN topic.                                                                   |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | restart_when_exception  | No              | Boolean          | Whether to enable the function of restart upon exceptions.                                                                                                              |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resume_checkpoint       | No              | Boolean          | Whether to restore data from the latest checkpoint when the system automatically restarts upon an exception. The default value is **false**.                            |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resume_max_num          | No              | Integer          | Maximum retry attempts. **-1** indicates there is no upper limit.                                                                                                       |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_path         | No              | String           | Path for saving the checkpoint.                                                                                                                                         |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | idle_state_retention    | No              | Integer          | Expiration time.                                                                                                                                                        |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | config_url              | No              | String           | OBS path of the **config** package uploaded by the user.                                                                                                                |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | udf_jar_url             | No              | String           | Name of the package that has been uploaded to the DLI resource management system. The **UDF Jar** file of the SQL job is uploaded through this parameter.               |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dirty_data_strategy     | No              | String           | Dirty data policy of a job.                                                                                                                                             |
   |                         |                 |                  |                                                                                                                                                                         |
   |                         |                 |                  | -  **2:obsDir**: Save. **obsDir** specifies the path for storing dirty data.                                                                                            |
   |                         |                 |                  | -  **1**: Trigger a job exception                                                                                                                                       |
   |                         |                 |                  | -  **0**: Ignore                                                                                                                                                        |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | entrypoint              | No              | String           | Name of the package that has been uploaded to the DLI resource management system. This parameter is used to customize the JAR file where the job main class is located. |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dependency_jars         | No              | Array of Strings | Name of the package that has been uploaded to the DLI resource management system. This parameter is used to customize other dependency packages.                        |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dependency_files        | No              | Array of Strings | Name of the resource package that has been uploaded to the DLI resource management system. This parameter is used to customize dependency files.                        |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_cus                  | No              | int              | Number of CUs per TaskManager node.                                                                                                                                     |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_slot_num             | No              | int              | Number of slots per TaskManager node.                                                                                                                                   |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | operator_config         | No              | String           | Operator's parallelism degree. The operator ID and degree of parallelism are displayed in JSON format.                                                                  |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | static_estimator_config | No              | String           | Estimation of static flow diagram resources.                                                                                                                            |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | runtime_config          | No              | String           | Customizes optimization parameters when a Flink job is running.                                                                                                         |
   +-------------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

-  The following example takes the **flink_jar_job** type as an example:

   .. code-block::

      {
          "is_success": "true",
          "message": "Job detail query succeeds.",
          "job_detail": {
              "job_id": 104,
              "user_id": "011c99a26ae84a1bb963a75e7637d3fd",
              "queue_name": "flinktest",
              "project_id": "330e068af1334c9782f4226acc00a2e2",
              "name": "jptest",
              "desc": "",
              "sql_body": "",
              "run_mode": "exclusive_cluster",
              "job_type": "flink_jar_job",
              "job_config": {
                  "checkpoint_enabled": false,
                  "checkpoint_interval": 10,
                  "checkpoint_mode": "exactly_once",
                  "log_enabled": false,
                  "obs_bucket": null,
                  "root_id": -1,
                  "edge_group_ids": null,
                  "graph_editor_enabled": false,
                  "graph_editor_data": "",
                  "manager_cu_number": 1,
                  "executor_number": null,
                  "executor_cu_number": null,
                  "cu_number": 2,
                  "parallel_number": 1,
                  "smn_topic": null,
                  "restart_when_exception": false,
                  "idle_state_retention": 3600,
                  "config_url": null,
                  "udf_jar_url": null,
                  "dirty_data_strategy": null,
                  "entrypoint": "FemaleInfoCollection.jar",
                  "dependency_jars": [
                      "FemaleInfoCollection.jar",
                      "ObsBatchTest.jar"
                  ],
                  "dependency_files": [
                      "FemaleInfoCollection.jar",
                      "ReadFromResource"
                  ]
              },
              "main_class": null,
              "entrypoint_args": null,
              "execution_graph": null,
              "status": "job_init",
              "status_desc": "",
              "create_time": 1578466221525,
              "update_time": 1578467395713,
              "start_time": null
          }
      }

Status Codes
------------

:ref:`Table 5 <dli_02_0235__table181259166119>` describes the status code.

.. _dli_02_0235__table181259166119:

.. table:: **Table 5** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         Querying details of a job succeeds.
   400         The input parameter is invalid.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
