:original_name: dli_01_0650.html

.. _dli_01_0650:

Setting the Priority for a Flink Job
====================================

Scenario
--------

In actual job running, it is necessary to prioritize and ensure the normal running of important and urgent tasks due to their varying levels of importance and urgency. This requires providing the necessary compute resources for their normal operations.

DLI offers a feature to set job priorities for each Flink job, which prioritizes the allocation of compute resources to higher priority jobs when resources are limited.

.. note::

   You can set the priority for Flink 1.12 or later jobs.

Notes
-----

-  You can assign a priority level of 1 to 10 for each job, with a larger value indicating a higher priority. Compute resources are preferentially allocated to high-priority jobs. That is, if compute resources required for high-priority jobs are insufficient, compute resources for low-priority jobs are reduced.
-  Flink jobs running on a general-purpose queue have a default priority level of 5.
-  The job priority change will only be in effect once the job has been stopped, edited, and resubmitted.
-  You can set the priority for Flink jobs only after enabling dynamic scaling by setting **flink.dli.job.scale.enable** to **true**. For details, see :ref:`Enabling Dynamic Scaling for Flink Jobs <dli_01_0534>`.
-  To change the priority for a job, you must first stop the job, change the priority level, and then submit the job for the modification to take effect.

Setting the Priority for a Flink OpenSource SQL Job
---------------------------------------------------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.

#. Select the job for which you want to set the priority and click **Edit** in the **Operation** column.

#. On the far right of the displayed page, click **Runtime Configuration**.

#. Enter statements in the text box to enable dynamic scaling and then set the job priority.

   .. note::

      To set the priority for Flink jobs, you must first enable dynamic scaling by setting **flink.dli.job.scale.enable** to **true**.

      For more parameter settings, see :ref:`Enabling Dynamic Scaling for Flink Jobs <dli_01_0534>`.

   .. code-block::

      flink.dli.job.scale.enable=true
      flink.dli.job.priority=x

Setting the Priority for a Flink Jar Job
----------------------------------------

Enter the following statement in the **Runtime Configuration** text box, where *x* indicates the priority value:

.. code-block::

   flink.dli.job.priority=x

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.

#. Select the job for which you want to set the priority and click **Edit** in the **Operation** column.

#. In the **Runtime Configuration** text box, enter the following statements to enable dynamic scaling and set the job priority:

   .. note::

      To set the priority for Flink jobs, you must first enable dynamic scaling by setting **flink.dli.job.scale.enable** to **true**.

      For more parameter settings, see :ref:`Enabling Dynamic Scaling for Flink Jobs <dli_01_0534>`.

   .. code-block::

      flink.dli.job.scale.enable=true
      flink.dli.job.priority=x
