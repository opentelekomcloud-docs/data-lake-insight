:original_name: dli_03_0276.html

.. _dli_03_0276:

How Can I Check the Actual and Used CUs for an Elastic Resource Pool as Well as the Required CUs for a Job?
===========================================================================================================

In daily big data analysis work, it is important to allocate and manage compute resources properly to provide a good job execution environment.

You can allocate resources and adjust task execution order based on the job's compute needs and data scale, and schedule different elastic resource pools or queues to adapt to different workloads. To ensure normal job execution, the CUs required for the submitted job should be less than or equal to the remaining available CUs in the elastic resource pool.

This section describes how to view the usage of compute resources in an elastic resource pool and the required CUs for a job.

Checking the Actual and Used CUs for an Elastic Resource Pool
-------------------------------------------------------------

#. Log in to the DLI management console.

#. Choose **Resources** > **Resource Pool**.

   Locate the target resource pool in the list and check its **Actual CUs** and **Used CUs**.

   -  **Actual CUs**: number of CUs that can be allocated in the elastic resource pool.
   -  **Used CUs**: CUs that have been allocated to and used by the current elastic resource pool.

   To ensure normal job execution, the CUs required for the submitted job should be less than or equal to the remaining available CUs in the elastic resource pool.

   For details about the number of CUs required by different types of jobs, see :ref:`Checking the Required CUs for a Job <dli_03_0276__en-us_topic_0000001835345441_section1235018713154>`.

.. _dli_03_0276__en-us_topic_0000001835345441_section1235018713154:

Checking the Required CUs for a Job
-----------------------------------

-  **SQL job**:

   Use the monitoring dashboard provided by Cloud Eye to check the number of running and submitted jobs, and use the job count to determine the overall resource usage of SQL jobs.

-  **Flink job**:

   #. Log in to the DLI management console.

   #. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.

   #. In the job list, click the name of the target job.

   #. Click **Flink Job Settings** then **Resources**.

   #. Check the value of **CUs**, that is, the total number of CUs used by the job.

      You can set the number of CUs on the job editing page using the following formula: CUs = Job Manager CUs + (Parallelism/Slots per TM) x CUs per TM.

-  **Spark job**:

   #. Log in to the DLI management console.

   #. In the navigation pane on the left, choose **Job Management** > **Spark Jobs**.

   #. Locate the target job in the list and click **Edit** in the **Operation** column.

      Check the compute resource specifications configured for the job.

      The formula is as follows:

      Number of CUs of a Spark job = Number of CUs used by executors + Number of CUs used by the driver

      Number of CUs used by executors = max {[(Executors x Executor Memory)/4], (Executors x Executor Cores)} x 1

      Number of CUs used by the driver = max [(Driver Memory/4), Driver Cores] x 1

      .. note::

         -  If **Advanced Settings** is set to **Skip** for a Spark job, resource specifications of type A are used by default.
         -  The unit of compute resource specifications for Spark jobs is CU. One CU consists of one CPU and 4 GB of memory. In the formulas above, x1 represents the conversion of CPU to CU.
         -  To calculate the required CUs for the executors or driver, use either the memory or the number of CPU cores. Choose the larger value between the two as the number of required CUs.
