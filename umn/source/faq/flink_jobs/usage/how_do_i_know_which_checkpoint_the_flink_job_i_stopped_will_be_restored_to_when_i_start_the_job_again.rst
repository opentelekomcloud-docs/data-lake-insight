:original_name: dli_03_0180.html

.. _dli_03_0180:

How Do I Know Which Checkpoint the Flink Job I Stopped Will Be Restored to When I Start the Job Again?
======================================================================================================

Symptom
-------

Checkpoint was enabled when a Flink job is created, and the OBS bucket for storing checkpoints was specified. After a Flink job is manually stopped, no message is displayed specifying the checkpoint where the Flink job will be restored if the Flink job is started again.

Solution
--------

The generation mechanism and format of Flink checkpoints are the same as those of savepoints. You can import a savepoint of the job to restore it from the latest checkpoint saved in OBS.

#. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
#. Locate the row that contains the target Flink job, and click **Import Savepoint** in the **Operation** column.
#. In the displayed dialog box, select the OBS bucket path storing the checkpoint. The checkpoint save path is *Bucket name*\ **/jobs/checkpoint/**\ *directory starting with the job ID*. Click **OK**.
#. Start the Flink job again. The job will be restored fom the imported savepoint.
