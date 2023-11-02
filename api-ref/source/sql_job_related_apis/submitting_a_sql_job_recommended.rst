:original_name: dli_02_0102.html

.. _dli_02_0102:

Submitting a SQL Job (Recommended)
==================================

Function
--------

This API is used to submit jobs to a queue using SQL statements.

The job types support DDL, DCL, IMPORT, QUERY, and INSERT. The IMPORT function is the same as that described in :ref:`Importing Data <dli_02_0019>`. The difference lies in the implementation method.

Additionally, you can use other APIs to query and manage jobs. For details, see the following sections:

-  :ref:`Querying Job Status <dli_02_0021>`
-  :ref:`Querying Job Details <dli_02_0022>`
-  :ref:`Exporting Query Results <dli_02_0024>`
-  :ref:`Querying All Jobs <dli_02_0025>`
-  :ref:`Canceling a Job (Recommended) <dli_02_0104>`

.. note::

   This API is synchronous if **job_type** in the response message is **DCL**.

URI
---

-  URI format

   POST /v1.0/{project_id}/jobs/submit-job

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

   +------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                                                                                                                                                                      |
   +============+===========+==================+==================================================================================================================================================================================================+
   | sql        | Yes       | String           | SQL statement that you want to execute.                                                                                                                                                          |
   +------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | currentdb  | No        | String           | Database where the SQL statement is executed. This parameter does not need to be configured during database creation.                                                                            |
   +------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name | No        | String           | Name of the queue to which a job to be submitted belongs. The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).        |
   +------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | conf       | No        | Array of Strings | You can set the configuration parameters for the SQL job in the form of **Key/Value**. For details about the supported configuration items, see :ref:`Table 3 <dli_02_0102__table334825142314>`. |
   +------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags       | No        | Array of Objects | Label of a job. For details, see :ref:`Table 4 <dli_02_0102__table9391124139>`.                                                                                                                  |
   +------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0102__table334825142314:

.. table:: **Table 3** Configuration parameters description

   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                   | Default Value         | Description                                                                                                                                                                                                                                                                                                                                                                                               |
   +=============================================+=======================+===========================================================================================================================================================================================================================================================================================================================================================================================================+
   | spark.sql.files.maxRecordsPerFile           | 0                     | Maximum number of records to be written into a single file. If the value is zero or negative, there is no limit.                                                                                                                                                                                                                                                                                          |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.sql.autoBroadcastJoinThreshold        | 209715200             | Maximum size of the table that displays all working nodes when a connection is executed. You can set this parameter to **-1** to disable the display.                                                                                                                                                                                                                                                     |
   |                                             |                       |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                             |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                             |                       |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                             |                       |    Currently, only the configuration unit metastore table that runs the **ANALYZE TABLE COMPUTE statistics noscan** command and the file-based data source table that directly calculates statistics based on data files are supported.                                                                                                                                                                   |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.sql.shuffle.partitions                | 200                   | Default number of partitions used to filter data for join or aggregation.                                                                                                                                                                                                                                                                                                                                 |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.sql.dynamicPartitionOverwrite.enabled | false                 | Whether DLI overwrites the partitions where data will be written into during runtime. If you set this parameter to **false**, all partitions that meet the specified condition will be deleted before data overwrite starts. For example, if you set **false** and use INSERT OVERWRITE to write partition 2021-02 to a partitioned table that has the 2021-01 partition, this partition will be deleted. |
   |                                             |                       |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                             |                       | If you set this parameter to **true**, DLI does not delete partitions before overwrite starts.                                                                                                                                                                                                                                                                                                            |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.sql.files.maxPartitionBytes           | 134217728             | Maximum number of bytes to be packed into a single partition when a file is read.                                                                                                                                                                                                                                                                                                                         |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.sql.badRecordsPath                    | ``-``                 | Path of bad records.                                                                                                                                                                                                                                                                                                                                                                                      |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dli.sql.sqlasync.enabled                    | false                 | Indicates whether DDL and DCL statements are executed asynchronously. The value **true** indicates that asynchronous execution is enabled.                                                                                                                                                                                                                                                                |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dli.sql.job.timeout                         | ``-``                 | Sets the job running timeout interval. If the timeout interval expires, the job is canceled. Unit: second                                                                                                                                                                                                                                                                                                 |
   +---------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0102__table9391124139:

.. table:: **Table 4** tags parameters

   ========= ========= ====== ===========
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========
   key       Yes       String Tag key.
   value     Yes       String Tag value
   ========= ========= ====== ===========

Response
--------

.. table:: **Table 5** Response parameters

   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                      |
   +=================+=================+==================+==================================================================================================================================================+
   | is_success      | Yes             | Boolean          | Indicates whether the request is successfully sent. Value **true** indicates that the request is successfully sent.                              |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | message         | Yes             | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                                                   |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_id          | Yes             | String           | ID of a job returned after a job is generated and submitted by using SQL statements. The job ID can be used to query the job status and results. |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_type        | Yes             | String           | Type of a job. Job types include the following:                                                                                                  |
   |                 |                 |                  |                                                                                                                                                  |
   |                 |                 |                  | -  DDL                                                                                                                                           |
   |                 |                 |                  | -  DCL                                                                                                                                           |
   |                 |                 |                  | -  IMPORT                                                                                                                                        |
   |                 |                 |                  | -  EXPORT                                                                                                                                        |
   |                 |                 |                  | -  QUERY                                                                                                                                         |
   |                 |                 |                  | -  INSERT                                                                                                                                        |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema          | No              | Array of objects | If the statement type is DDL, the column name and type of DDL are displayed.                                                                     |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | rows            | No              | Array of objects | When the statement type is DDL, results of the DDL are displayed.                                                                                |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_mode        | No              | String           | Job execution mode. The options are as follows:                                                                                                  |
   |                 |                 |                  |                                                                                                                                                  |
   |                 |                 |                  | -  **async**: asynchronous                                                                                                                       |
   |                 |                 |                  | -  **sync**: synchronous                                                                                                                         |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Submit a SQL job. The job execution database and queue are **db1** and **default**, respectively. Then, add the tags **workspace=space1** and **jobName=name1** for the job.

.. code-block::

   {
       "currentdb": "db1",
       "sql": "desc table1",
       "queue_name": "default",
       "conf": [
           "dli.sql.shuffle.partitions = 200"
       ],
       "tags": [
               {
                 "key": "workspace",
                 "value": "space1"
                },
               {
                 "key": "jobName",
                 "value": "name1"
                }
         ]
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "job_id": "8ecb0777-9c70-4529-9935-29ea0946039c",
     "job_type": "DDL",
     "job_mode":"sync",
     "schema": [
       {
         "col_name": "string"
       },
       {
         "data_type": "string"
       },
       {
         "comment": "string"
       }
     ],
     "rows": [
       [
         "c1",
         "int",
         null
       ],
       [
         "c2",
         "string",
         null
       ]
     ]
   }

Status Codes
------------

:ref:`Table 6 <dli_02_0102__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0102__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 6** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Submitted successfully.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
