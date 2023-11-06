:original_name: dli_01_0535.html

.. _dli_01_0535:

Setting the Priority for a Job
==============================

Scenario
--------

In actual job operations, jobs have different importance and urgency levels. So, the compute resources required for normal operations of important and urgent jobs need to be guaranteed.

DLI allows you to set the priority of each Spark job, Spark SQL job, and Flink job. When compute resources are insufficient, the resources are preferentially used for jobs with higher priorities.

.. note::

   -  Priorities can only be set for jobs running in elastic resource pools.
   -  SQL jobs in elastic resource pools support priorities.
   -  Priorities can be set for jobs of Spark 2.4.5 or later.
   -  Priorities can be set for jobs of Flink 1.12 or later.

Notes
-----

-  You can set the priority for each job. The value ranges from 1 to 10. A larger value indicates a higher priority. Compute resources are preferentially allocated to high-priority jobs. That is, if compute resources required for high-priority jobs are insufficient, compute resources for low-priority jobs are reduced.
-  The default priority of Flink jobs running on a general-purpose queue is 5.
-  The default priority of Spark jobs running on a general-purpose queue is 3.
-  The default priority of jobs running on a SQL queue is 3.
-  The change to the job priority takes effect only after the job is stopped, edited, and submitted.
-  For Flink jobs, enable dynamic scaling by setting **flink.dli.job.scale.enable** to **true** by referring to :ref:`Enabling Dynamic Scaling for Flink Jobs <dli_01_0534>`, and then set the job priority.
-  The change to the priority for a Flink job takes effect only after the job is stopped, edited, and submitted.

Procedure for Setting the Priorities for Flink OpenSource SQL Jobs
------------------------------------------------------------------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.

#. Locate the row containing the job for which you want to set the priority and click **Edit** in the **Operation** column.

#. On the right of the page displayed, click **Runtime Configuration**.

#. In the text box, enter the following statements to enable dynamic scaling and then set the job priority:

   .. note::

      For Flink jobs, you must set **flink.dli.job.scale.enable** to **true** to enable dynamic scaling, and then set the job priority.

      For more parameter settings for enabling dynamic scaling, see :ref:`Enabling Dynamic Scaling for Flink Jobs <dli_01_0534>`.

   .. code-block::

      flink.dli.job.scale.enable=true
      flink.dli.job.priority=x

Procedure for Setting the Priorities for Flink Jar Jobs
-------------------------------------------------------

In the **Runtime Configuration** text box, enter the following statement. *x* indicates the priority value.

.. code-block::

   flink.dli.job.priority=x

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.

#. Locate the row containing the job for which you want to set the priority and click **Edit** in the **Operation** column.

#. In the **Runtime Configuration** text box, enter the following statements to enable dynamic scaling and set the job priority:

   .. note::

      For Flink jobs, you must set **flink.dli.job.scale.enable** to **true** to enable dynamic scaling, and then set the job priority.

      For more parameter settings for enabling dynamic scaling, see :ref:`Enabling Dynamic Scaling for Flink Jobs <dli_01_0534>`.

   .. code-block::

      flink.dli.job.scale.enable=true
      flink.dli.job.priority=x

Procedure for Setting the Priorities for Spark Jobs
---------------------------------------------------

In the **Spark Arguments(--conf)** text box, enter the following statement. *x* indicates the priority value.

.. code-block::

   spark.dli.job.priority=x

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Job Management** > **Spark Jobs**.
#. Locate the row containing the job for which you want to set the priority and click **Edit** in the **Operation** column.
#. In the **Spark Arguments(--conf)** text box, configure the **spark.dli.job.priority** parameter.

Procedure for Setting the Priorities for Spark SQL Jobs
-------------------------------------------------------

Click **Settings**. In the **Parameter Settings** area, configure the following parameter. *x* indicates the priority value.

.. code-block::

   spark.sql.dli.job.priority=x

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Job Management** > **SQL Jobs**.
#. Locate the row containing the job for which you want to set the priority and click **Edit** in the **Operation** column.
#. Click **Settings**. In the **Parameter Settings** area, configure the **spark.sql.dli.job.priority** parameter.
