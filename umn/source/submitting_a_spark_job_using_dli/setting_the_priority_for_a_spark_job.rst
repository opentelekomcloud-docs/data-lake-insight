:original_name: dli_01_0652.html

.. _dli_01_0652:

Setting the Priority for a Spark Job
====================================

Scenario
--------

In actual job running, it is necessary to prioritize and ensure the normal running of important and urgent tasks due to their varying levels of importance and urgency. This requires providing the necessary compute resources for their normal operations.

DLI offers a feature to set job priorities for each Spark job, which prioritizes the allocation of compute resources to higher priority jobs when resources are limited.

.. note::

   You can set the priority for Spark 2.4.5 or later jobs.

Notes
-----

-  You can assign a priority level of 1 to 10 for each job, with a larger value indicating a higher priority. Compute resources are preferentially allocated to high-priority jobs. That is, if compute resources required for high-priority jobs are insufficient, compute resources for low-priority jobs are reduced.
-  Spark jobs running on a general-purpose queue have a default priority level of 3.
-  To change the priority for a job, you must first stop the job, change the priority level, and then submit the job for the modification to take effect.

Procedure
---------

Enter the following statement in the **Spark Arguments(--conf)** text box, where *x* indicates the priority value:

.. code-block::

   spark.dli.job.priority=x

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Job Management** > **Spark Jobs**.
#. Select the job for which you want to set the priority and click **Edit** in the **Operation** column.
#. Set the **spark.dli.job.priority** parameter in the **Spark Arguments(--conf)** text box.
