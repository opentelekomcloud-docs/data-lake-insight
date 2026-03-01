:original_name: dli_02_0231.html

.. _dli_02_0231:

Updating a Flink Jar Job
========================

Function
--------

This API is used to update custom jobs, which currently support the JAR format and run in dedicated queues.

URI
---

-  URI format

   PUT /v1.0/{*project_id*}/streaming/flink-jobs/{*job_id*}

-  Parameter descriptions

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                      |
      +============+===========+========+==================================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain a project ID, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | Long   | Job ID. Refer to :ref:`Creating a Flink Jar Job <dli_02_0230>` to obtain the value.                                                              |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Parameter descriptions

   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory       | Type             | Description                                                                                                                                                                                   |
   +=========================+=================+==================+===============================================================================================================================================================================================+
   | name                    | No              | String           | Job name. Length range: 0 to 57 characters.                                                                                                                                                   |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | desc                    | No              | String           | Job description. Length range: 0 to 512 characters.                                                                                                                                           |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name              | No              | String           | Queue name. Length range: 1 to 128 characters.                                                                                                                                                |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cu_number               | No              | Integer          | Number of CUs selected for a job. The default value is **2**.                                                                                                                                 |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | manager_cu_number       | No              | Integer          | Number of CUs on the management node selected by the user for a job, which corresponds to the number of Flink job managers. The default value is **1**.                                       |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parallel_number         | No              | Integer          | Number of parallel operations selected for a job. The default value is **1**.                                                                                                                 |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_enabled      | No              | Boolean          | Whether to enable the automatic job snapshot function. Options:                                                                                                                               |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | -  **true**: Enable this function.                                                                                                                                                            |
   |                         |                 |                  | -  **false**: Do not enable this function.                                                                                                                                                    |
   |                         |                 |                  | -  Default value: **false**.                                                                                                                                                                  |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_mode         | No              | Integer          | Snapshot mode. Options:                                                                                                                                                                       |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | -  **1**: **ExactlyOnce**, meaning data is consumed only once.                                                                                                                                |
   |                         |                 |                  | -  **2**: **AtLeastOnce**, meaning data is consumed at least once.                                                                                                                            |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | Default value: **1**.                                                                                                                                                                         |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_interval     | No              | Integer          | Snapshot interval. The unit is second. The default value is **10**.                                                                                                                           |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | log_enabled             | No              | Boolean          | Whether to enable the job log function. Options:                                                                                                                                              |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | -  **true**: Enable this function.                                                                                                                                                            |
   |                         |                 |                  | -  **false**: Do not enable this function.                                                                                                                                                    |
   |                         |                 |                  | -  Default value: **false**.                                                                                                                                                                  |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_bucket              | No              | String           | OBS bucket where users are authorized to save logs when **log_enabled** is set to **true**.                                                                                                   |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | smn_topic               | No              | String           | SMN topic. If a job fails, the system will send a message to users subscribed to this SMN topic.                                                                                              |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | main_class              | No              | String           | Job entry class.                                                                                                                                                                              |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | entrypoint_args         | No              | String           | Job entry parameter. Multiple parameters are separated by spaces.                                                                                                                             |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | restart_when_exception  | No              | Boolean          | Whether to enable the function of restart upon exceptions. The default value is **false**.                                                                                                    |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | entrypoint              | No              | String           | Name of the package that has been uploaded to the DLI resource management system. This parameter is used to customize the JAR file where the job main class is located.                       |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | For Flink 1.15 or later, you can only select packages from OBS, instead of DLI.                                                                                                               |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | Example: **obs://bucket_name/test.jar**                                                                                                                                                       |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dependency_jars         | No              | Array of strings | Name of the package that has been uploaded to the DLI resource management system. This parameter is used to customize other dependency packages.                                              |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | For Flink 1.15 or later, you can only select packages from OBS, instead of DLI.                                                                                                               |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | Example: **obs://bucket_name/test1.jar, obs://bucket_name/test2.jar**                                                                                                                         |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dependency_files        | No              | Array of strings | Name of the resource package that has been uploaded to the DLI resource management system. This parameter is used to customize dependency files.                                              |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | For Flink 1.15 or later, you can only select packages from OBS, instead of DLI.                                                                                                               |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | Example: **[obs://bucket_name/file1, obs://bucket_name/file2]**                                                                                                                               |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_cus                  | No              | Integer          | Number of CUs for each TaskManager. The default value is **1**.                                                                                                                               |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_slot_num             | No              | Integer          | Number of slots in each TaskManager. The default value is **(parallel_number*tm_cus)/(cu_number-manager_cu_number)**.                                                                         |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | execution_agency_urn    | No              | String           | Name of the agency authorized to DLI. This parameter is configurable in Flink 1.15.                                                                                                           |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resume_checkpoint       | No              | Boolean          | Whether the abnormal restart is recovered from the checkpoint.                                                                                                                                |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resume_max_num          | No              | Integer          | Maximum number of retry times upon exceptions. The unit is times/hour. Value range: -1 or greater than 0. The default value is **-1**, indicating that the number of times is unlimited.      |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_path         | No              | String           | Storage address of the checkpoint in the JAR file of the user. The path must be unique.                                                                                                       |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | runtime_config          | No              | String           | Customizes optimization parameters when a Flink job is running.                                                                                                                               |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_type                | No              | String           | Job type. Options:                                                                                                                                                                            |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | **flink_jar_job**: User-defined Flink job                                                                                                                                                     |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_config_version | No              | String           | Resource configuration version. The value can be **v1** or **v2**. The default value is **v1**.                                                                                               |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | Compared with the v1 template, the v2 template does not support the setting of the number of CUs. The v2 template supports the setting of **Job Manager Memory** and **Task Manager Memory**. |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | **v1**: applicable to Flink 1.12 and 1.15.                                                                                                                                                    |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | **v2**: applicable to Flink 1.15 and 1.17.                                                                                                                                                    |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | You are advised to use the parameter settings of v2.                                                                                                                                          |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_config         | No              | Object           | Resource configuration of a Flink job. For detailed parameter descriptions, refer to :ref:`Table 3 <dli_02_0231__table193993254472>`.                                                         |
   |                         |                 |                  |                                                                                                                                                                                               |
   |                         |                 |                  | When the resource configuration version is **v2**, the configuration takes effect; when the resource configuration version is **v1**, the configuration is invalid.                           |
   +-------------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0231__table193993254472:

.. table:: **Table 3** resource_config parameters

   +---------------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                 | Mandatory       | Type            | Description                                                                                                                                                                                                                                  |
   +===========================+=================+=================+==============================================================================================================================================================================================================================================+
   | max_slot                  | No              | integer         | Number of parallel tasks that a single TaskManager can support. Each task slot can execute one task in parallel. Increasing task slots enhances the parallel processing capacity of the TaskManager but also increases resource consumption. |
   |                           |                 |                 |                                                                                                                                                                                                                                              |
   |                           |                 |                 | The number of task slots is linked to the CPU count of the TaskManager since each CPU can offer one task slot.                                                                                                                               |
   |                           |                 |                 |                                                                                                                                                                                                                                              |
   |                           |                 |                 | By default, a single TM slot is set to **1**. The minimum parallelism must not be less than 1.                                                                                                                                               |
   +---------------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parallel_number           | No              | integer         | Number of tasks concurrently executed by each operator in a job. The default value is **1**.                                                                                                                                                 |
   +---------------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | jobmanager_resource_spec  | No              | Object          | JobManager resource specifications. For details about the parameters, see :ref:`Table 4 <dli_02_0231__table7152833178>`.                                                                                                                     |
   +---------------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | taskmanager_resource_spec | No              | Object          | TaskManager resource specifications. For details about the parameters, see :ref:`Table 5 <dli_02_0231__table138972571488>`.                                                                                                                  |
   +---------------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0231__table7152833178:

.. table:: **Table 4** jobmanager_resource_spec parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                         |
   +=================+=================+=================+=====================================================================================================================================================================================================================+
   | cpu             | No              | double          | Number of CPU cores available for JobManager. The default value is **1.0** CPU core, with a minimum of no less than 0.5 CPU cores.                                                                                  |
   |                 |                 |                 |                                                                                                                                                                                                                     |
   |                 |                 |                 | If the current job is running on a basic edition elastic resource pool (16-64 CUs), it is recommended that the JobManager's CPU value does not exceed 2 to avoid resource scheduling failures during job execution. |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | memory          | No              | string          | Memory that can be used by JobManager, in MB or GB. The default unit is GB. The default value is 4 GB, and the minimum value is 2 GB.                                                                               |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0231__table138972571488:

.. table:: **Table 5** taskmanager_resource_spec parameters

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                          |
   +=================+=================+=================+======================================================================================================================================================================================================================+
   | cpu             | No              | double          | Number of CPU cores available for TaskManager. The default value is **1.0** CPU core, with a minimum of no less than 0.5 CPU cores.                                                                                  |
   |                 |                 |                 |                                                                                                                                                                                                                      |
   |                 |                 |                 | If the current job is running on a basic edition elastic resource pool (16-64 CUs), it is recommended that the TaskManager's CPU value does not exceed 2 to avoid resource scheduling failures during job execution. |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | memory          | No              | string          | Memory that can be used by TaskManager, in MB or GB. The default unit is GB. The default value is 4 GB, and the minimum value is 2 GB.                                                                               |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

.. table:: **Table 6** Response parameters

   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                 |
   +============+===========+========+=============================================================================================================+
   | is_success | No        | String | Whether the request is successfully executed. **true** indicates that the request is successfully executed. |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------+
   | message    | No        | String | Message content.                                                                                            |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------+
   | job        | No        | object | Information about job update. For details, see :ref:`Table 7 <dli_02_0231__table128621016345>`.             |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------+

.. _dli_02_0231__table128621016345:

.. table:: **Table 7** job parameter

   +-------------+-----------+------+------------------------------------------------------+
   | Parameter   | Mandatory | Type | Description                                          |
   +=============+===========+======+======================================================+
   | update_time | No        | Long | Time when a job is updated. The unit is millisecond. |
   +-------------+-----------+------+------------------------------------------------------+

Example Request
---------------

Update the Flink Jar job information. After the update, the job name is **test1**, the job execution queue is **testQueue**, and the job log function is disabled.

.. code-block::

   {
       "name": "test1",
       "desc": "job for test",
       "job_type": "flink_jar_job",
       "queue_name": "testQueue",
       "manager_cu_number": 1,
       "cu_number": 2,
       "parallel_number": 1,
       "log_enabled": false,
       "main_class": "org.apache.flink.examples.streaming.JavaQueueStream",
       "restart_when_exception": false,
       "entrypoint": "FemaleInfoCollec.jar",
       "dependency_jars": [
           "myGroup/test.jar",
           "myGroup/test1.jar"
       ],
       "execution_agency_urn": "myAgencyName",
       "dependency_files": [
           "myGroup/test.csv",
           "myGroup/test1.csv"
       ]
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "The Flink job is updated successfully.",
     "job": {
        "update_time": 1516952770835
     }
   }

Status Codes
------------

:ref:`Table 8 <dli_02_0231__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0231__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 8** Status codes

   =========== =============================================
   Status Code Description
   =========== =============================================
   200         The custom Flink job is updated successfully.
   400         The input parameter is invalid.
   =========== =============================================

Error Codes
-----------

If an error occurs when this API is called, the system does not return the result similar to the preceding example, but returns an error code and error message. For details, see :ref:`Error Codes <dli_02_0056>`.
