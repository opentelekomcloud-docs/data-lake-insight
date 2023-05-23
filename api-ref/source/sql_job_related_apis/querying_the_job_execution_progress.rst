:original_name: dli_02_0296.html

.. _dli_02_0296:

Querying the Job Execution Progress
===================================

Function
--------

This API is used to obtain the job execution progress. If a job is being executed, information about its subjobs can be obtained. If a job has just started or has ended, information about its subjobs cannot be obtained.

URI
---

-  URI format

   GET /v1/{project_id}/jobs/{job_id}/progress

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | String | Job ID                                                                                                                                        |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                                 |
   +=================+=================+=================+=============================================================================================================================================================================================================================================================================================================================+
   | is_success      | Yes             | Boolean         | Indicates whether the request is successfully sent. Value **true** indicates that the request is successfully sent.                                                                                                                                                                                                         |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message         | Yes             | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                                                                                                                                                                                                                              |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_id          | No              | String          | ID of a job returned after a job is generated and submitted by using SQL statements. The job ID can be used to query the job status and results.                                                                                                                                                                            |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | status          | Yes             | String          | Job status. The status can be **RUNNING**, **SCALING**, **LAUNCHING**, **FINISHED**, **FAILED**, or **CANCELLED**.                                                                                                                                                                                                          |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sub_job_id      | No              | Integer         | ID of a subjob that is running. If the subjob is not running or it is already finished, the subjob ID may be empty.                                                                                                                                                                                                         |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | progress        | No              | Double          | Progress of a running subjob or the entire job. The value can only be a rough estimate of the subjob progress and does not indicate the detailed job progress.                                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | -  If the job is just started or being submitted, the progress is displayed as **0**. If the job execution is complete, the progress is displayed as **1**. In this case, progress indicates the running progress of the entire job. Because no subjob is running, **sub_job_id** is not displayed.                         |
   |                 |                 |                 | -  If a subjob is running, the running progress of the subjob is displayed. The calculation method of progress is as follows: Number of completed tasks of the subjob/Total number of tasks of the subjob. In this case, progress indicates the running progress of the subjob, and **sub_job_id** indicates the subjob ID. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sub_jobs        | No              | Array of Object | Details about a subjob of a running job. A job may contain multiple subjobs. For details, see :ref:`Table 3 <dli_02_0296__table2091820139203>`.                                                                                                                                                                             |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0296__table2091820139203:

.. table:: **Table 3** Parameters in the **sub_jobs** field

   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory | Type                | Description                                                                                                                        |
   +=======================+===========+=====================+====================================================================================================================================+
   | id                    | No        | Integer             | Subjob ID, corresponding to **jobId** of the open-source spark JobData.                                                            |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | name                  | No        | String              | Subjob name, corresponding to the **name** of the open-source spark JobData.                                                       |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | description           | No        | String              | Description of a subjob, corresponding to the **description** of the open-source spark JobData.                                    |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | submission_time       | No        | String              | Submission time of a subjob, corresponding to the **submissionTime** of open-source Spark JobData.                                 |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | completion_time       | No        | String              | Completion time of a subjob, corresponding to the **completionTime** of the open-source Spark JobData.                             |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | stage_ids             | No        | Array of Integer    | Stage ID of the subjob, corresponding to the **stageIds** of the open-source spark JobData.                                        |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | job_group             | No        | String              | ID of a DLI job, corresponding to the **jobGroup** of open-source Spark JobData.                                                   |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | status                | No        | String              | Subjob status, corresponding to the **status** of open-source spark JobData.                                                       |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_tasks             | No        | Integer             | Number of subjobs, corresponding to **numTasks** of the open-source Spark JobData.                                                 |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_active_tasks      | No        | Integer             | Number of running tasks in a subjob, corresponding to **numActiveTasks** of the open-source Spark JobData.                         |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_completed_tasks   | No        | Integer             | Number of tasks that have been completed in a subjob, corresponding to **numCompletedTasks** of open-source Spark JobData.         |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_skipped_tasks     | No        | Integer             | Number of tasks skipped in a subjob, corresponding to **numSkippedTasks** of open-source Spark JobData.                            |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_failed_tasks      | No        | Integer             | Number of subtasks that fail to be skipped, corresponding to **numFailedTasks** of open-source Spark JobData.                      |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_killed_tasks      | No        | Integer             | Number of tasks killed in the subjob, corresponding to **numKilledTasks** of the open-source Spark JobData.                        |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_completed_indices | No        | Integer             | Subjob completion index, corresponding to the **numCompletedIndices** of the open-source Spark JobData.                            |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_active_stages     | No        | Integer             | Number of stages that are running in the subjob, corresponding to **numActiveStages** of the open-source Spark JobData.            |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_completed_stages  | No        | Integer             | Number of stages that have been completed in the subjob, corresponding to **numCompletedStages** of the open-source Spark JobData. |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_skipped_stages    | No        | Integer             | Number of stages skipped in the subjob, corresponding to **numSkippedStages** of the open-source Spark JobData.                    |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | num_failed_stages     | No        | Integer             | Number of failed stages in a subjob, corresponding to **numFailedStages** of the open-source Spark JobData.                        |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | killed_tasks_summary  | No        | Map<string,integer> | Summary of the killed tasks in the subjob, corresponding to **killedTasksSummary** of open-source spark JobData.                   |
   +-----------------------+-----------+---------------------+------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "",
       "job_id": "85798b38-ae44-48eb-bb90-7cf0dcdafe7b",
       "status": "RUNNING",
       "sub_job_id": 0,
       "progress": 0,
       "sub_jobs": [
           {
               "id": 0,
               "name": "runJob at FileFormatWriter.scala:266",
               "submission_time": "Mon Jul 27 17:24:03 CST 2020",
               "stage_ids": [
                   0
               ],
               "job_group": "85798b38-ae44-48eb-bb90-7cf0dcdafe7b",
               "status": "RUNNING",
               "num_tasks": 1,
               "num_active_tasks": 1,
               "num_completed_tasks": 0,
               "num_skipped_tasks": 0,
               "num_failed_tasks": 0,
               "num_killed_tasks": 0,
               "num_completed_indices": 0,
               "num_active_stages": 1,
               "num_completed_stages": 0,
               "num_skipped_stages": 0,
               "num_failed_stages": 0
           }
       ]
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0296__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0296__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ========================
   Status Code Description
   =========== ========================
   200         The query is successful.
   400         Request error.
   =========== ========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.

.. table:: **Table 5** Error codes

   ========== ==========================================================
   Error Code Error Message
   ========== ==========================================================
   DLI.0999   The queue backend version is too old or the queue is busy.
   ========== ==========================================================
