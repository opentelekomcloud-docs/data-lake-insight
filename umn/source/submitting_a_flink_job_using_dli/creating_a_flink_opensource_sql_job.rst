:original_name: dli_01_0498.html

.. _dli_01_0498:

Creating a Flink OpenSource SQL Job
===================================

This section describes how to create a Flink OpenSource SQL job.

DLI Flink OpenSource SQL jobs are fully compatible with the syntax of Flink provided by the community. In addition, Redis and GaussDB(DWS) data source types are added based on the community connector. For the syntax and constraints of Flink SQL DDL, DML, and functions, see `Table API & SQL <https://ci.apache.org/projects/flink/flink-docs-release-1.10/dev/table/sql/>`__.

Prerequisites
-------------

-  You have prepared the source and sink streams.
-  A datasource connection has been created to enable the network between the queue where the job is about to run and external data sources.

   -  For details about the external data sources that can be accessed by Flink jobs, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

   -  For how to create a datasource connection, see :ref:`Configuring the Network Connection Between DLI and Data Sources (Enhanced Datasource Connection) <dli_01_0426>`.

      On the **Resources** > **Queue Management** page, locate the queue you have created, click **More** in the **Operation** column, and select **Test Address Connectivity** to check if the network connection between the queue and the data source is normal. For details, see :ref:`Testing Address Connectivity <dli_01_0489>`.


Creating a Flink OpenSource SQL Job
-----------------------------------

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. In the upper right corner of the **Flink Jobs** page, click **Create Job**.

#. Set job parameters.

   .. table:: **Table 1** Job parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                         |
      +===================================+=====================================================================================================================================================================================================================================================================================================================+
      | Type                              | Set **Type** to **Flink OpenSource SQL**. You will need to start jobs by compiling SQL statements.                                                                                                                                                                                                                  |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                              | Job name. Enter 1 to 57 characters. Only letters, numbers, hyphens (-), and underscores (_) are allowed.                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |    The job name must be globally unique.                                                                                                                                                                                                                                                                            |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                       | Description of a job. It can contain a maximum of 512 characters.                                                                                                                                                                                                                                                   |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Template Name                     | You can select a sample template or a custom job template. For details about templates, see :ref:`Managing Flink Job Templates <dli_01_0464>`.                                                                                                                                                                      |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tags                              | Tags used to identify cloud resources. A tag includes the tag key and tag value. If you want to use the same tag to identify multiple cloud resources, that is, to select the same tag from the drop-down list box for all services, you are advised to create predefined tags on the Tag Management Service (TMS). |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |    -  A maximum of 20 tags can be added.                                                                                                                                                                                                                                                                            |
      |                                   |    -  Only one tag value can be added to a tag key.                                                                                                                                                                                                                                                                 |
      |                                   |    -  The key name in each resource must be unique.                                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  Tag key: Enter a tag key name in the text box.                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |       A tag key can contain a maximum of 128 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                                  |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  Tag value: Enter a tag value in the text box.                                                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |       A tag value can contain a maximum of 255 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed.                                                                                                                                                                          |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK** to enter the editing page.

#. Edit an OpenSource SQL job.

   Enter detailed SQL statements in the statement editing area. For details about SQL statements, see the *Data Lake Insight Flink OpenSource SQL Syntax Reference*.

#. Click **Check Semantics**.

   -  You can **Start** a job only after the semantic verification is successful.
   -  If verification is successful, the message "The SQL semantic verification is complete. No error." will be displayed.
   -  If verification fails, a red "X" mark will be displayed in front of each SQL statement that produced an error. You can move the cursor to the "X" mark to view error details and change the SQL statement as prompted.

   .. note::

      Flink 1.15 does not support syntax verification.

#. Set job running parameters.

   .. table:: **Table 2** Running parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                        |
      +===================================+====================================================================================================================================================================================================================================================+
      | Queue                             | Select a queue to run the job.                                                                                                                                                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | UDF Jar                           | UDF JAR file, which contains UDFs that can be called in subsequent jobs.                                                                                                                                                                           |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | There are the following ways to manage UDF JAR files:                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | -  Upload packages to OBS: Upload Jar packages to an OBS bucket in advance and select the corresponding OBS path.                                                                                                                                  |
      |                                   | -  Upload packages to DLI: Upload JAR files to an OBS bucket in advance and create a package on the **Data Management** > **Package Management** page of the DLI management console. For details, see :ref:`Creating a DLI Package <dli_01_0367>`. |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | For Flink 1.15 or later, only OBS packages can be selected when creating jobs, and DLI packages are not supported.                                                                                                                                 |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Agency                            | If you choose Flink 1.15 to execute your job, you can create a custom agency to allow DLI to access other services.                                                                                                                                |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | CUs                               | Sum of the number of compute units and job manager CUs of DLI. One CU equals 1 vCPU and 4 GB.                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | The value is the number of CUs required for job running and cannot exceed the number of CUs in the bound queue.                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | .. note::                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    When **Task Manager Config** is selected, elastic resource pool queue management is optimized by automatically adjusting **CUs** to match **Actual CUs** after setting **Slot(s) per TM**.                                                      |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    **CUs = Actual number of CUs = max[Job Manager CPUs + Task Manager CPU, (Job Manager Memory + Task Manager Memory/4)]**                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    -  Job Manager CPUs + Task Manager CPUs = Actual TMs x CU(s) per TM + Job Manager CUs.                                                                                                                                                          |
      |                                   |    -  Job Manager Memory + Task Manager Memory = Actual TMs x Memory per TM + Job Manager Memory                                                                                                                                                   |
      |                                   |    -  If **Slot(s) per TM** is set, then: Actual TMs = Parallelism/Slot(s) per TM.                                                                                                                                                                 |
      |                                   |    -  If **Slot(s) per TM** is not set, then: Actual TMs = (CUs - Job Manager CUs)/CU(s) per TM.                                                                                                                                                   |
      |                                   |    -  If **Memory per TM** and **Job Manager Memory** in the optimization parameters are not set, then: Memory per TM = CU(s) per TM x 4. Job Manager Memory = Job Manager CUs x 4.                                                                |
      |                                   |    -  The parallelism degree of Spark resources is jointly determined by the number of Executors and the number of Executor CPU cores.                                                                                                             |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Job Manager CUs                   | Number of CUs of the management unit.                                                                                                                                                                                                              |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parallelism                       | Number of tasks concurrently executed by each operator in a job.                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | .. note::                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    This value cannot be greater than four times the compute units (number of CUs minus the number of job manager CUs).                                                                                                                             |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Task Manager Config               | Whether Task Manager resource parameters are set                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | -  If selected, you need to set the following parameters:                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    -  **CU(s) per TM**: Number of resources occupied by each Task Manager.                                                                                                                                                                         |
      |                                   |    -  **Slot(s) per TM**: Number of slots contained in each Task Manager.                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | -  If not selected, the system automatically uses the default values.                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    -  **CU(s) per TM**: The default value is **1**.                                                                                                                                                                                                |
      |                                   |    -  **Slot(s) per TM**: The default value is (Parallelism x CU(s) per TM)/(CUs - Job Manager CUs).                                                                                                                                               |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | OBS Bucket                        | OBS bucket to store job logs and checkpoint information. If the OBS bucket you selected is unauthorized, click **Authorize**.                                                                                                                      |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Save Job Log                      | Whether job running logs are saved to OBS. The logs are saved in the following path: *Bucket name*\ **/jobs/logs/**\ *Directory starting with the job ID*.                                                                                         |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | .. caution::                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    CAUTION:                                                                                                                                                                                                                                        |
      |                                   |    You are advised to configure this parameter. Otherwise, no run log is generated after the job is executed. If the job fails, the run log cannot be obtained for fault locating.                                                                 |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | If this option is selected, you need to set the following parameters:                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | **OBS Bucket**: Select an OBS bucket to store job logs. If the OBS bucket you selected is unauthorized, click **Authorize**.                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | .. note::                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    If **Enable Checkpointing** and **Save Job Log** are both selected, you only need to authorize OBS once.                                                                                                                                        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm on Job Exception            | Whether to notify users of any job exceptions, such as running exceptions or arrears, via SMS or email.                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | If this option is selected, you need to set the following parameters:                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | **SMN Topic**                                                                                                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | Select a custom SMN topic. For how to create a custom SMN topic, see "Creating a Topic" in the *Simple Message Notification User Guide*.                                                                                                           |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Enable Checkpointing              | Whether to enable job snapshots. If this function is enabled, jobs can be restored based on the checkpoints.                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | If this option is selected, you need to set the following parameters:                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | -  **Checkpoint Interval**: interval for creating checkpoints, in seconds. The value ranges from 1 to 999999, and the default value is **30**.                                                                                                     |
      |                                   | -  **Checkpoint Mode** can be set to either of the following values:                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    -  **At least once**: Events are processed at least once.                                                                                                                                                                                       |
      |                                   |    -  **Exactly once**: Events are processed only once.                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | If you select **Enable Checkpointing**, you also need to set **OBS Bucket**.                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | **OBS Bucket**: Select an OBS bucket to store your checkpoints. If the OBS bucket you selected is unauthorized, click **Authorize**.                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | The checkpoint path is *Bucket name*\ **/jobs/checkpoint/**\ *Directory starting with the job ID*.                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | .. note::                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    If **Enable Checkpointing** and **Save Job Log** are both selected, you only need to authorize OBS once.                                                                                                                                        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Auto Restart upon Exception       | Whether automatic restart is enabled. If enabled, jobs will be automatically restarted and restored when exceptions occur.                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | If this option is selected, you need to set the following parameters:                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | -  **Max. Retry Attempts**: maximum number of retries upon an exception. The unit is times/hour.                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   |    -  **Unlimited**: The number of retries is unlimited.                                                                                                                                                                                           |
      |                                   |    -  **Limited**: The number of retries is user-defined.                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | -  **Restore Job from Checkpoint**: This parameter is available only when **Enable Checkpointing** is selected.                                                                                                                                    |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Idle State Retention Time         | Clears intermediate states of operators such as **GroupBy**, **RegularJoin**, **Rank**, and **Depulicate** that have not been updated after the maximum retention time. The default value is 1 hour.                                               |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Dirty Data Policy                 | Policy for processing dirty data. The following policies are supported: **Ignore**, **Trigger a job exception**, and **Save**.                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | If you set this field to **Save**, the **Dirty Data Dump Address** must be set. Click the address box to select the OBS path for storing dirty data.                                                                                               |
      |                                   |                                                                                                                                                                                                                                                    |
      |                                   | This parameter is available only when a DIS data source is used.                                                                                                                                                                                   |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. (Optional) Set the runtime configuration as required.

#. Click **Save**.

#. Click **Start**. On the displayed **Start Flink Jobs** page, confirm the job specifications, and click **Start Now** to start the job.

   After the job is started, the system automatically switches to the **Flink Jobs** page, and the created job is displayed in the job list. You can view the job status in the **Status** column. Once a job is successfully submitted, its status changes from **Submitting** to **Running**. After the execution is complete, the status changes to **Completed**.

   If the job status is **Submission failed** or **Running exception**, the job fails to submit or run. In this case, you can hover over the status icon in the **Status** column of the job list to view the error details. You can click |image1| to copy these details. Rectify the fault based on the error information and resubmit the job.

   .. note::

      Other buttons are as follows:

      -  **Save As**: Save the created job as a new job.
      -  **Static Stream Graph**: Provide the static concurrency estimation function and stream graph display function.
      -  **Simplified Stream Graph**: Display the data processing flow from the source to the sink.
      -  **Format**: Format the SQL statements in the editing box.
      -  **Set as Template**: Set the created SQL statements as a job template.
      -  **Theme Settings**: Set the theme related parameters, including **Font Size**, **Wrap**, and **Page Style**.

Simplified Stream Graph
-----------------------

On the OpenSource SQL job editing page, click **Simplified Stream Graph**.

.. note::

   Simplified stream graph viewing is only supported in Flink 1.12 and Flink 1.10.

Static Stream Graph
-------------------

On the OpenSource SQL job editing page, click **Static Stream Graph**.

.. note::

   -  Simplified stream graph viewing is only supported in Flink 1.12 and Flink 1.10.
   -  If you use a UDF in a Flink OpenSource SQL job, it is not possible to generate a static stream graph.

The **Static Stream Graph** page also allows you to:

-  Estimate concurrencies. Click **Estimate Concurrencies** on the **Static Stream Graph** page to estimate concurrencies. Click **Restore Initial Value** to restore the initial value after concurrency estimation.
-  Zoom in or out the page.
-  Expand or merge operator chains.
-  You can edit **Parallelism**, **Output rate**, and **Rate factor**.

   -  **Parallelism**: indicates the number of concurrent tasks.
   -  **Output rate**: indicates the data traffic of an operator. The unit is piece/s.
   -  **Rate factor**: indicates the retention rate after data is processed by operators. Rate factor = Data output volume of an operator/Data input volume of the operator (Unit: %)

.. |image1| image:: /_static/images/en-us_image_0000001078931615.png
