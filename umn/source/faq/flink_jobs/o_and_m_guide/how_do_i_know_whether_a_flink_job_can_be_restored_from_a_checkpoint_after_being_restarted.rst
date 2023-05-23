:original_name: dli_03_0136.html

.. _dli_03_0136:

How Do I Know Whether a Flink Job Can Be Restored from a Checkpoint After Being Restarted?
==========================================================================================

Check the following operations:

-  Adjusting or adding optimization parameters or the number of concurrent threads of a job, or modifying Flink SQL statements or a Flink Jar job: The job cannot be restored from the checkpoint.
-  Modifying the number of CUs occupied by a TaskManager: The job can be restored from the checkpoint.
