:original_name: dli_03_0136.html

.. _dli_03_0136:

How Can I Check if a Flink Job Can Be Restored From a Checkpoint After Restarting It?
=====================================================================================

What Is Restoration from a Checkpoint?
--------------------------------------

Flink's checkpointing is a fault tolerance and recovery mechanism. This mechanism ensures that real-time programs can self-recover in case of exceptions or machine issues during runtime.

.. _dli_03_0136__en-us_topic_0000001116262956_section145423915149:

Principles for Restoration from Checkpoints
-------------------------------------------

-  When a job fails to be executed or a resource restarts due to an exception that is not triggered by manual operations, data can be restored from a checkpoint.
-  However, if the calculation logic of a job is modified, the job cannot be restored from a checkpoint.

Application Scenarios
---------------------

:ref:`Table 1 <dli_03_0136__en-us_topic_0000001116262956_table145380549145>` lists some common scenarios of restoring data from a checkpoint for your reference.

For more scenarios, refer to :ref:`Principles for Restoration from Checkpoints <dli_03_0136__en-us_topic_0000001116262956_section145423915149>` and assess whether data can be restored from a checkpoint based on the actual situation.

.. _dli_03_0136__en-us_topic_0000001116262956_table145380549145:

.. table:: **Table 1** Common scenarios of restoring data from a checkpoint

   +------------------------------------------------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Scenario                                                   | Restoration from a Checkpoint | Description                                                                                                                                                                                                   |
   +============================================================+===============================+===============================================================================================================================================================================================================+
   | Adjust or increase the number of concurrent tasks.         | Not supported                 | This operation alters the parallelism of the job, thereby changing its execution logic.                                                                                                                       |
   +------------------------------------------------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Modify Flink SQL statements and Flink Jar jobs.            | Not supported                 | This operation modifies the algorithmic logic of the job with respect to resources.                                                                                                                           |
   |                                                            |                               |                                                                                                                                                                                                               |
   |                                                            |                               | For example, if the original algorithm involves addition and subtraction, but the desired state requires multiplication, division, and modulo operations, it cannot be restored directly from the checkpoint. |
   +------------------------------------------------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Modify the static stream graph.                            | Not supported                 | This operation modifies the algorithmic logic of the job with respect to resources.                                                                                                                           |
   +------------------------------------------------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Modify the **CU(s) per TM** parameter.                     | Supported                     | The modification of compute resources does not affect the operational logic of the job's algorithm or operators.                                                                                              |
   +------------------------------------------------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A job runs abnormally or there is a physical power outage. | Supported                     | The job parameters are not modified.                                                                                                                                                                          |
   +------------------------------------------------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
