:original_name: dli_01_0462.html

.. _dli_01_0462:

Flink Job Details
=================

After creating a job, you can view the job details to learn about the following information:

-  :ref:`Viewing Job Details <dli_01_0462__section18786181516319>`
-  :ref:`Checking Job Monitoring Information <dli_01_0462__section93781115103311>`
-  :ref:`Viewing the Task List of a Job <dli_01_0462__section11677164916529>`
-  :ref:`Viewing the Job Execution Plan <dli_01_0462__section9397163320>`
-  :ref:`Viewing Job Submission Logs <dli_01_0462__section18556377020>`
-  :ref:`Viewing Job Running Logs <dli_01_0462__section91816183316>`

.. _dli_01_0462__section18786181516319:

Viewing Job Details
-------------------

This section describes how to view job details. After you create and save a job, you can click the job name to view job details, including SQL statements and parameter settings. For a Jar job, you can only view its parameter settings.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Click the name of the job to be viewed. The **Job Detail** tab is displayed.

   In the **Job Details** tab, you can view SQL statements, configured parameters.

   The following uses a Flink SQL job as an example.

   .. table:: **Table 1** Description

      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                           | Description                                                                                                                          |
      +=====================================+======================================================================================================================================+
      | Type                                | Job type, for example, **Flink SQL**                                                                                                 |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Name                                | Flink job name                                                                                                                       |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Description                         | Description of a Flink job                                                                                                           |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Status                              | Running status of a job                                                                                                              |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Running Mode                        | If your job runs on a shared queue, this parameter is **Shared**.                                                                    |
      |                                     |                                                                                                                                      |
      |                                     | If your job runs on a custom queue with dedicated resources, this parameter is **Exclusive**.                                        |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Flink Version                       | Version of Flink selected for the job.                                                                                               |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Runtime Configuration               | Displayed when a user-defined parameter is added to a job                                                                            |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | CUs                                 | Number of CUs configured for a job                                                                                                   |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Job Manager CUs                     | Number of job manager CUs configured for a job.                                                                                      |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Parallelism                         | Number of jobs that can be concurrently executed by a Flink job                                                                      |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | CU(s) per TM                        | Number of CUs occupied by each Task Manager configured for a job                                                                     |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Slot(s) per TM                      | Number of Task Manager slots configured for a job                                                                                    |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | OBS Bucket                          | OBS bucket name. After **Enable Checkpointing** and **Save Job Log** are enabled, checkpoints and job logs are saved in this bucket. |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Save Job Log                        | Whether the job running logs are saved to OBS                                                                                        |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm Generation upon Job Exception | Whether job exceptions are reported                                                                                                  |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | SMN Topic                           | Name of the SMN topic. This parameter is displayed when **Alarm Generation upon Job Exception** is enabled.                          |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Auto Restart upon Exception         | Whether automatic restart is enabled.                                                                                                |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Max. Retry Attempts                 | Maximum number of retry times upon an exception. **Unlimited** means the number is not limited.                                      |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Savepoint                           | OBS path of the savepoint                                                                                                            |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Enable Checkpointing                | Whether checkpointing is enabled                                                                                                     |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Checkpoint Interval                 | Interval between storing intermediate job running results to OBS. The unit is second.                                                |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Checkpoint Mode                     | Checkpoint mode. Available values are as follows:                                                                                    |
      |                                     |                                                                                                                                      |
      |                                     | -  **At least once**: Events are processed at least once.                                                                            |
      |                                     | -  **Exactly once**: Events are processed only once.                                                                                 |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Idle State Retention Time           | Defines for how long the state of a key is retained without being updated before it is removed in GroupBy or Window.                 |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Dirty Data Policy                   | Policy for processing dirty data. The value is displayed only when there is a dirty data policy. Available values are as follows:    |
      |                                     |                                                                                                                                      |
      |                                     | **Ignore**                                                                                                                           |
      |                                     |                                                                                                                                      |
      |                                     | **Trigger a job exception**                                                                                                          |
      |                                     |                                                                                                                                      |
      |                                     | **Save**                                                                                                                             |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Dirty Data Dump Address             | OBS path for storing dirty data when **Dirty Data Policy** is set to **Save**.                                                       |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Created                             | Time when a job is created                                                                                                           |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Updated                             | Time when a job was last updated                                                                                                     |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0462__section93781115103311:

Checking Job Monitoring Information
-----------------------------------

You can use Cloud Eye to view details about job data input and output.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Click the name of the job you want. The job details are displayed.

   Click **Job Monitoring** in the upper right corner of the page to switch to the Cloud Eye console.

   The following table describes monitoring metrics related to Flink jobs.

   .. table:: **Table 2** Monitoring metrics related to Flink jobs

      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Name                                    | Description                                                                                                     |
      +=========================================+=================================================================================================================+
      | Flink Job Data Read Rate                | Displays the data input rate of a Flink job for monitoring and debugging. Unit: record/s.                       |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Data Write Rate               | Displays the data output rate of a Flink job for monitoring and debugging. Unit: record/s.                      |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Total Data Read               | Displays the total number of data inputs of a Flink job for monitoring and debugging. Unit: records             |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Total Data Write              | Displays the total number of output data records of a Flink job for monitoring and debugging. Unit: records     |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Byte Read Rate                | Displays the number of input bytes per second of a Flink job. Unit: byte/s                                      |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Byte Write Rate               | Displays the number of output bytes per second of a Flink job. Unit: byte/s                                     |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Total Read Byte               | Displays the total number of input bytes of a Flink job. Unit: byte                                             |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Total Write Byte              | Displays the total number of output bytes of a Flink job. Unit: byte                                            |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job CPU Usage                     | Displays the CPU usage of Flink jobs. Unit: %                                                                   |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Memory Usage                  | Displays the memory usage of Flink jobs. Unit: %                                                                |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Max Operator Latency          | Displays the maximum operator delay of a Flink job. The unit is **ms**.                                         |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+
      | Flink Job Maximum Operator Backpressure | Displays the maximum operator backpressure value of a Flink job. A larger value indicates severer backpressure. |
      |                                         |                                                                                                                 |
      |                                         | **0**: OK                                                                                                       |
      |                                         |                                                                                                                 |
      |                                         | **50**: low                                                                                                     |
      |                                         |                                                                                                                 |
      |                                         | **100**: high                                                                                                   |
      +-----------------------------------------+-----------------------------------------------------------------------------------------------------------------+

.. _dli_01_0462__section11677164916529:

Viewing the Task List of a Job
------------------------------

You can view details about each task running on a job, including the task start time, number of received and transmitted bytes, and running duration.

.. note::

   If the value is **0**, no data is received from the data source.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Click the name of the job you want. The job details are displayed.

#. On **Task List** and view the node information about the task.

   View the operator task list. The following table describes the task parameters.

   .. table:: **Table 3** Parameter description

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                 |
      +===================================+=============================================================================================================================================+
      | Name                              | Name of an operator.                                                                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Duration                          | Running duration of an operator.                                                                                                            |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Max Concurrent Jobs               | Number of parallel tasks in an operator.                                                                                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Task                              | Operator tasks are categorized as follows:                                                                                                  |
      |                                   |                                                                                                                                             |
      |                                   | -  The digit in red indicates the number of failed tasks.                                                                                   |
      |                                   | -  The digit in light gray indicates the number of canceled tasks.                                                                          |
      |                                   | -  The digit in yellow indicates the number of tasks that are being canceled.                                                               |
      |                                   | -  The digit in green indicates the number of finished tasks.                                                                               |
      |                                   | -  The digit in blue indicates the number of running tasks.                                                                                 |
      |                                   | -  The digit in sky blue indicates the number of tasks that are being deployed.                                                             |
      |                                   | -  The digit in dark gray indicates the number of tasks in a queue.                                                                         |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Status                            | Status of an operator task.                                                                                                                 |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Back Pressure Status              | Working load status of an operator. Available options are as follows:                                                                       |
      |                                   |                                                                                                                                             |
      |                                   | -  **OK**: indicates that the operator is in normal working load.                                                                           |
      |                                   | -  **LOW**: indicates that the operator is in slightly high working load. DLI processes data quickly.                                       |
      |                                   | -  **HIGH**: indicates that the operator is in high working load. The data input speed at the source end is slow.                           |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Delay                             | Duration from the time when source data starts being processed to the time when data reaches the current operator. The unit is millisecond. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Sent Records                      | Number of data records sent by an operator.                                                                                                 |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Sent Bytes                        | Number of bytes sent by an operator.                                                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Received Bytes                    | Number of bytes received by an operator.                                                                                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Received Records                  | Number of data records received by an operator.                                                                                             |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Started                           | Time when an operator starts running.                                                                                                       |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Ended                             | Time when an operator stops running.                                                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0462__section9397163320:

Viewing the Job Execution Plan
------------------------------

You can view the execution plan to understand the operator stream information about the running job.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Click the name of the job you want. The job details are displayed.

#. Click the **Execution Plan** tab to view the operator flow direction.

   Click a node. The corresponding information is displayed on the right of the page.

   -  Scroll the mouse wheel to zoom in or out.
   -  The stream diagram displays the operator stream information about the running job in real time.

.. _dli_01_0462__section18556377020:

Viewing Job Submission Logs
---------------------------

You can view the submission logs to locate the fault.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.
#. Click the name of the job you want. The job details are displayed.
#. In the **Commit Logs** tab, view the information about the job submission process.

.. _dli_01_0462__section91816183316:

Viewing Job Running Logs
------------------------

You can view the run logs to locate the faults occurring during job running.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Click the name of the job you want. The job details are displayed.

#. On the **Run Log** tab page, you can view the **Job Manager** and **Task Manager** information of the running job.

   Information about JobManager and TaskManager is updated every minute. Only run logs of the last minute are displayed by default.

   If you select an OBS bucket for saving job logs during the job configuration, you can switch to the OBS bucket and download log files to view more historical logs.

   If the job is not running, information on the **Task Manager** page cannot be viewed.
