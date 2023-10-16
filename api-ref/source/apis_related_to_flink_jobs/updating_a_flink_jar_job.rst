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

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | Long   | Job ID. Refer to :ref:`Creating a Flink Jar job <dli_02_0230>` to obtain the value.                                                           |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Parameter description

   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Mandatory       | Type             | Description                                                                                                                                                                              |
   +========================+=================+==================+==========================================================================================================================================================================================+
   | name                   | No              | String           | Name of the job. Length range: 0 to 57 characters.                                                                                                                                       |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | desc                   | No              | String           | Job description. Length range: 0 to 512 characters.                                                                                                                                      |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name             | No              | String           | Name of a queue. Length range: 1 to 128 characters.                                                                                                                                      |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cu_number              | No              | Integer          | Number of CUs selected for a job. The default value is **2**.                                                                                                                            |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | manager_cu_number      | No              | Integer          | Number of CUs on the management node selected by the user for a job, which corresponds to the number of Flink job managers. The default value is **1**.                                  |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parallel_number        | No              | Integer          | Number of parallel operations selected for a job. The default value is **1**.                                                                                                            |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | log_enabled            | No              | Boolean          | Whether to enable the job log function.                                                                                                                                                  |
   |                        |                 |                  |                                                                                                                                                                                          |
   |                        |                 |                  | -  **true**: indicates to enable the job log function.                                                                                                                                   |
   |                        |                 |                  | -  **false**: indicates to disable the job log function.                                                                                                                                 |
   |                        |                 |                  | -  Default value: **false**                                                                                                                                                              |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_bucket             | No              | String           | OBS path where users are authorized to save logs when **log_enabled** is set to **true**.                                                                                                |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | smn_topic              | No              | String           | SMN topic. If a job fails, the system will send a message to users subscribed to the SMN topic.                                                                                          |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | main_class             | No              | String           | Job entry class.                                                                                                                                                                         |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | entrypoint_args        | No              | String           | Job entry parameter. Multiple parameters are separated by spaces.                                                                                                                        |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | restart_when_exception | No              | Boolean          | Whether to enable the function of restart upon exceptions. The default value is **false**.                                                                                               |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | entrypoint             | No              | String           | Name of the package that has been uploaded to the DLI resource management system. This parameter is used to customize the JAR file where the job main class is located.                  |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dependency_jars        | No              | Array of Strings | Name of the package that has been uploaded to the DLI resource management system. This parameter is used to customize other dependency packages.                                         |
   |                        |                 |                  |                                                                                                                                                                                          |
   |                        |                 |                  | Example: **myGroup/test.jar,myGroup/test1.jar**.                                                                                                                                         |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dependency_files       | No              | Array of Strings | Name of the resource package that has been uploaded to the DLI resource management system. This parameter is used to customize dependency files.                                         |
   |                        |                 |                  |                                                                                                                                                                                          |
   |                        |                 |                  | Example: **myGroup/test.cvs,myGroup/test1.csv**.                                                                                                                                         |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_cus                 | No              | Integer          | Number of CUs for each TaskManager. The default value is **1**.                                                                                                                          |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tm_slot_num            | No              | Integer          | Number of slots in each TaskManager. The default value is **(parallel_number*tm_cus)/(cu_number-manager_cu_number)**.                                                                    |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resume_checkpoint      | No              | Boolean          | Whether the abnormal restart is recovered from the checkpoint.                                                                                                                           |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resume_max_num         | No              | Integer          | Maximum number of retry times upon exceptions. The unit is times/hour. Value range: -1 or greater than 0. The default value is **-1**, indicating that the number of times is unlimited. |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_path        | No              | String           | Storage address of the checkpoint in the JAR file of the user. The path must be unique.                                                                                                  |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | runtime_config         | No              | String           | Customizes optimization parameters when a Flink job is running.                                                                                                                          |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_type               | No              | String           | Job types.                                                                                                                                                                               |
   +------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | Message content.                                                                                                  |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | job        | No        | object  | Information about job update. For details, see :ref:`Table 4 <dli_02_0231__table128621016345>`.                   |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0231__table128621016345:

.. table:: **Table 4** **job** parameters

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

:ref:`Table 5 <dli_02_0231__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0231__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 5** Status codes

   =========== =============================================
   Status Code Description
   =========== =============================================
   200         The custom Flink job is updated successfully.
   400         The input parameter is invalid.
   =========== =============================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
