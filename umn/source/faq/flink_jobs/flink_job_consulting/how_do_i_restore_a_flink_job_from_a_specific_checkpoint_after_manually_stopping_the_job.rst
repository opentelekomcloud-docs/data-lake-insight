:original_name: dli_03_0180.html

.. _dli_03_0180:

How Do I Restore a Flink Job from a Specific Checkpoint After Manually Stopping the Job?
========================================================================================

Symptom
-------

Checkpoint was enabled when a Flink job is created, and the OBS bucket for storing checkpoints was specified. I am not sure how to restore a Flink job from a specific checkpoint after manually stopping the job.

Solution
--------

Since the Flink checkpoint and savepoint generation mechanisms and formats are consistent, you can restore the Flink job from the latest successful checkpoint in OBS. Specifically, in the Flink job list, locate the desired Flink job, click **More** in the **Operation** column, and select **Import Savepoint** to import the checkpoint.

#. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
#. Locate the row that contains the target Flink job, and click **Import Savepoint** in the **Operation** column.
#. In the displayed dialog box, select the OBS bucket path storing the checkpoint. The checkpoint save path is *Bucket name*\ **/jobs/checkpoint/**\ *directory starting with the job ID*. Click **OK**.
#. Start the Flink job again. The job will be restored fom the imported savepoint.
