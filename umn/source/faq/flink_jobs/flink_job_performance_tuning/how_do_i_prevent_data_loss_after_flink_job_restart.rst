:original_name: dli_03_0096.html

.. _dli_03_0096:

How Do I Prevent Data Loss After Flink Job Restart?
===================================================

The DLI Flink checkpoint/savepoint mechanism is complete and reliable. You can use this mechanism to prevent data loss when a job is manually restarted or restarted due to an exception.

-  To prevent data loss caused by job restart due to system faults, perform the following operations:

   -  For Flink SQL jobs, select **Enable Checkpointing** and set a proper checkpoint interval that allows for the impact on service performance and the exception recovery duration. Select **Auto Restart upon Exception** and **Restore Job from Checkpoint**. After the configuration, if a job is restarted abnormally, the internal state and consumption position will be restored from the latest checkpoint file to ensure no data loss and accurate and consistent semantics of the internal state such as aggregation operators. In addition, to ensure that data is not duplicated, use a database or file system with a primary key as the data source. Otherwise, add deduplication logic (data generated from the latest successful checkpoint to the time exception occurred will be repeatedly consumed) for downstream processes.
   -  For Flink Jar jobs, you need to enable checkpointing in the code. Additionally, if you have a custom state to save, you need to implement the ListCheckpointed interface and set a unique ID for each operator. In the job configuration, select **Restore Job from Checkpoint** and configure the checkpoint path.

      .. note::

         Flink checkpointing ensures that the internal state data is accurate and consistent. However, for custom Source/Sink or stateful operators, you need to implement the ListCheckpointed API to ensure the reliability of service data.

-  To prevent data loss after a job is manually restarted due to service modification, perform the following operations:

   -  For jobs without internal states, you can set the start time or consumption position of the Kafka data source to a time before the job stops.
   -  For jobs with internal states, you can select **Trigger Savepoint** when stopping the job. Enable **Restore Savepoint** when you start the job again. The job will restore the consumption position and state from the selected savepoint file. The generation mechanism and format of Flink checkpoints are the same as those of savepoints. You can go to the Flink job list and choose **More** > **Import Savepoint** in the **Operation** column of a Flink job to import the latest checkpoint in OBS and restore the job from it.
