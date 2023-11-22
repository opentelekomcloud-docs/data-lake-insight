:original_name: dli_01_0461.html

.. _dli_01_0461:

Performing Operations on a Flink Job
====================================

After a job is created, you can perform operations on the job as required.

-  :ref:`Editing a Job <dli_01_0461__section1950210297542>`
-  :ref:`Starting a Job <dli_01_0461__section20957159163012>`
-  :ref:`Stopping a Job <dli_01_0461__section8678193324114>`
-  :ref:`Deleting a Job <dli_01_0461__section1691624195713>`
-  :ref:`Exporting a Job <dli_01_0461__section135831511323>`
-  :ref:`Importing a Job <dli_01_0461__section75781665389>`
-  :ref:`Modifying Name and Description <dli_01_0461__section15861321183619>`
-  :ref:`Importing to a Savepoint <dli_01_0461__section83412445175>`
-  :ref:`Triggering a Savepoint <dli_01_0461__section11401152191015>`
-  :ref:`Runtime Configuration <dli_01_0461__section48961252113110>`

.. _dli_01_0461__section1950210297542:

Editing a Job
-------------

You can edit a created job, for example, by modifying the SQL statement, job name, job description, or job configurations.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. In the row where the job you want to edit locates, click **Edit** in the **Operation** column to switch to the editing page.

#. Edit the job as required.

   For details about how to edit a Flink SQL job, see Step 5 to Step 7 in :ref:`Creating a Flink SQL Job <dli_01_0455>`.

   For details about how to edit a user-defined Flink job, see Step 5 to Step 7 in :ref:`Creating a Flink Jar Job <dli_01_0457>`.

.. _dli_01_0461__section20957159163012:

Starting a Job
--------------

You can start a saved or stopped job.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Use either of the following methods to start jobs:

   -  Starting a single job

      Select a job and click **Start** in the **Operation** column.

      Alternatively, you can select the row where the job you want to start locates and click **Start** in the upper left of the job list.

   -  Batch starting jobs

      Select the rows where the jobs you want to start locate and click **Start** in the upper left of the job list.

   After you click **Start**, the **Start Flink Jobs** page is displayed.

3. On the **Start Flink Jobs** page, confirm the job information. If they are correct, click **Start Now**.

   After a job is started, you can view the job execution result in the **Status** column.

.. _dli_01_0461__section8678193324114:

Stopping a Job
--------------

You can stop a job in the **Running** or **Submitting** state.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. Stop a job using either of the following methods:

   -  Stopping a job

      Locate the row that contains the job to be stopped, click **More** in the **Operation** column, and select **Stop**.

      Alternatively, you can select the row where the job you want to stop locates and click **Stop** in the upper left of the job list.

   -  Batch stopping jobs

      Locate the rows containing the jobs you want to stop and click **Stop** in the upper left of the job list.

#. In the displayed **Stop Job** dialog box, click **OK** to stop the job.

   .. note::

      -  Before stopping a job, you can trigger a savepoint to save the job status information. When you start the job again, you can choose whether to restore the job from the savepoint.
      -  If you select **Trigger savepoint**, a savepoint is created. If **Trigger savepoint** is not selected, no savepoint is created. By default, the savepoint function is disabled.
      -  The lifecycle of a savepoint starts when the savepoint is triggered and stops the job, and ends when the job is restarted. The savepoint is automatically deleted after the job is restarted.

   When a job is being stopped, the job status is displayed in the **Status** column of the job list. The details are as follows:

   -  **Stopping**: indicates that the job is being stopped.
   -  **Stopped**: indicates that the job is stopped successfully.
   -  **Stop failed**: indicates that the job failed to be stopped.

.. _dli_01_0461__section1691624195713:

Deleting a Job
--------------

If you do not need to use a job, perform the following operations to delete it. A deleted job cannot be restored. Therefore, exercise caution when deleting a job.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

2. Perform either of the following methods to delete jobs:

   -  Deleting a single job

      Locate the row containing the job you want to delete and click **More > Delete** in the **Operation** column.

      Alternatively, you can select the row containing the job you want to delete and click **Delete** in the upper left of the job list.

   -  Deleting jobs in batches

      Select the rows containing the jobs you want to delete and click **Delete** in the upper left of the job list.

3. Click **Yes**.

.. _dli_01_0461__section135831511323:

Exporting a Job
---------------

You can export the created Flink jobs to an OBS bucket.

This mode is applicable to the scenario where a large number of jobs need to be created when you switch to another region, project, or user. In this case, you do not need to create a job. You only need to export the original job, log in to the system in a new region or project, or use a new user to import the job.

.. note::

   When switching to another project or user, you need to grant permissions to the new project or user. For details, see :ref:`Managing Flink Job Permissions <dli_01_0479>`.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

2. Click **Export Job** in the upper right corner. The **Export Job** dialog box is displayed.

3. Select the OBS bucket where the job is stored. Click **Next**.

4. Select job information you want to export.

   By default, configurations of all jobs are exported. You can enable the **Custom Export** function to export configurations of the desired jobs.

5. Click **Confirm** to export the job.

.. _dli_01_0461__section75781665389:

Importing a Job
---------------

You can import the Flink job configuration file stored in the OBS bucket to the **Flink Jobs** page of DLI.

This mode is applicable to the scenario where a large number of jobs need to be created when you switch to another region, project, or user. In this case, you do not need to create a job. You only need to export the original job, log in to the system in a new region or project, or use a new user to import the job.

If you need to import a self-created job, you are advised to use the job creation function. For details, see :ref:`Creating a Flink SQL Job <dli_01_0455>` and :ref:`Creating a Flink Jar Job <dli_01_0457>`.

.. note::

   -  When switching to another project or user, you need to grant permissions to the new project or user. For details, see :ref:`Managing Flink Job Permissions <dli_01_0479>`.
   -  Only jobs whose data format is the same as that of Flink jobs exported from DLI can be imported.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

2. Click **Import Job** in the upper right corner. The **Import Job** dialog box is displayed.
3. Select the complete OBS path of the job configuration file to be imported. Click **Next**.
4. Configure the same-name job policy and click next. Click **Next**.

   -  Select **Overwrite job of the same name**. If the name of the job to be imported already exists, the existing job configuration will be overwritten and the job status switches to **Draft**.
   -  If **Overwrite job of the same name** is not selected and the name of the job to be imported already exists, the job will not be imported.

5. Ensure that **Config File** and **Overwrite Same-Name Job** are correctly configured. Click **Confirm** to import the job.

.. _dli_01_0461__section15861321183619:

Modifying Name and Description
------------------------------

You can change the job name and description as required.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.
#. In the **Operation** column of the job whose name and description need to be modified, choose **More > Modify Name and Description**. The **Modify Name and Description** dialog box is displayed. Change the name or modify the description of a job.
#. Click **OK**.

.. _dli_01_0461__section11401152191015:

Triggering a Savepoint
----------------------

When you need to stop a job, you can create a savepoint to save the job status information. In this case, when you restart the job, you can choose to restore the job from the latest savepoint.

.. note::

   -  You can click **Trigger Savepoint** for jobs in the **Running** status to save the job status.
   -  The lifecycle of a savepoint starts when the savepoint is triggered and stops the job, and ends when the job is restarted. The savepoint is automatically deleted after the job is restarted.

.. _dli_01_0461__section83412445175:

Importing to a Savepoint
------------------------

You can import a savepoint to restore the job status. For details about the savepoint, see `Checkpointing <https://nightlies.apache.org/flink/flink-docs-release-1.10/dev/stream/state/checkpointing.html>`__ at the official website of Flink.

You need to select the OBS path of the save point.

.. _dli_01_0461__section48961252113110:

Runtime Configuration
---------------------

You can select **Runtime Configuration** to configure job exception alarms and restart options.

.. note::

   Flink SQL jobs and Flink Jar jobs are supported.

#. In the **Operation** column of the Flink job, choose **More > Runtime Configuration**.
#. In the **Runtime Configuration** dialog box, set the following parameters:

   .. table:: **Table 1** Running parameters

      +-------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                           | Description                                                                                                                                                                                                                                          |
      +=====================================+======================================================================================================================================================================================================================================================+
      | Name                                | Job name.                                                                                                                                                                                                                                            |
      +-------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm Generation upon Job Exception | Whether to report job exceptions, for example, abnormal job running or exceptions due to an insufficient balance, to users via SMS or email.                                                                                                         |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     | **SMN Topic**                                                                                                                                                                                                                                        |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     | Select a user-defined SMN topic. For details about how to customize SMN topics, see **Creating a Topic** in the *Simple Message Notification User Guide*.                                                                                            |
      +-------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Auto Restart upon Exception         | Whether to enable automatic restart. If this function is enabled, any job that has become abnormal will be automatically restarted.                                                                                                                  |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     | -  **Max. Retry Attempts**: maximum number of retry times upon an exception. The unit is times/hour.                                                                                                                                                 |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     |    -  **Unlimited**: The number of retries is unlimited.                                                                                                                                                                                             |
      |                                     |    -  **Limited**: The number of retries is user-defined.                                                                                                                                                                                            |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     | -  **Restore Job from Checkpoint**: Restore the job from the saved checkpoint.                                                                                                                                                                       |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     |    .. note::                                                                                                                                                                                                                                         |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     |       This parameter cannot be configured for Flink SQL jobs or Flink OpenSource SQL jobs.                                                                                                                                                           |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     |    If this parameter is selected, you need to set **Checkpoint Path** for Flink Jar jobs.                                                                                                                                                            |
      |                                     |                                                                                                                                                                                                                                                      |
      |                                     |    **Checkpoint Path**: Select the checkpoint saving path. The checkpoint path must be the same as that you set in the application package. Note that the checkpoint path for each job must be unique. Otherwise, the checkpoint cannot be obtained. |
      +-------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
