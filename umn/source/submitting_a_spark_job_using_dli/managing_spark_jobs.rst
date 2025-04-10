:original_name: dli_01_0385.html

.. _dli_01_0385:

Managing Spark Jobs
===================

Viewing Basic Information
-------------------------

On the **Overview** page, click **Spark Jobs** to go to the SQL job management page. Alternatively, you can click **Job Management** > **Spark Jobs**. The page displays all Spark jobs. If there are a large number of jobs, they will be displayed on multiple pages. DLI allows you to view jobs in all statuses.

.. table:: **Table 1** Job management parameters

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                  |
   +===================================+==============================================================================================================+
   | Job ID                            | ID of a submitted Spark job, which is generated by the system by default.                                    |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Name                              | Name of a submitted Spark job.                                                                               |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Queues                            | Queue where the submitted Spark job runs                                                                     |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Username                          | Name of the user who executed the Spark job                                                                  |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Status                            | Job status. The following values are available:                                                              |
   |                                   |                                                                                                              |
   |                                   | -  **Starting**: The job is being started.                                                                   |
   |                                   | -  **Running**: The job is being executed.                                                                   |
   |                                   | -  **Failed**: The session has exited.                                                                       |
   |                                   | -  **Finished**: The session is successfully executed.                                                       |
   |                                   | -  **Restoring**: The job is being restored.                                                                 |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Created                           | Time when a job is created. Jobs can be displayed in ascending or descending order of the job creation time. |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Last Modified                     | Time when a job is completed.                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Edit**: You can modify the current job configuration and re-execute the job.                            |
   |                                   | -  **SparkUI**: After you click this button, the Spark job execution page is displayed.                      |
   |                                   |                                                                                                              |
   |                                   |    .. note::                                                                                                 |
   |                                   |                                                                                                              |
   |                                   |       -  The SparkUI page cannot be viewed for jobs in the **Starting** state.                               |
   |                                   |       -  Currently, only the latest 100 job information records are displayed on the SparkUI of DLI.         |
   |                                   |                                                                                                              |
   |                                   | -  **Terminate Job**: Cancel a job that is being started or running.                                         |
   |                                   | -  **Re-execute**: Run the job again.                                                                        |
   |                                   | -  **Archive Log**: Save job logs to the temporary bucket created by DLI.                                    |
   |                                   | -  **Commit Log**: View the logs of submitted jobs.                                                          |
   |                                   | -  **Driver Log**: View the logs of running jobs.                                                            |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------+

Re-executing a Job
------------------

On the **Spark Jobs** page, click **Edit** in the **Operation** column of the job. On the Spark job creation page that is displayed, modify parameters as required and execute the job.

Searching for a Job
-------------------

On the **Spark Jobs** page, select **Status** or **Queues**. The system displays the jobs that meet the filter condition in the job list.

Terminating a Job
-----------------

On the **Spark Jobs** page, choose **More** > **Terminate Job** in the **Operation** column of the job that you want to stop.
