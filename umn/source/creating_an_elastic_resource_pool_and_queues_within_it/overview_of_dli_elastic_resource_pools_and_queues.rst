:original_name: dli_01_0504.html

.. _dli_01_0504:

Overview of DLI Elastic Resource Pools and Queues
=================================================

DLI compute resources are the foundation to run jobs. This section describes the modes of DLI compute resources and queue types.

What Are Elastic Resource Pools and Queues?
-------------------------------------------

Before we dive into the compute resource modes of DLI, let us first understand the basic concepts of elastic resource pools and queues.

-  An **elastic resource pool** is a pooled management mode for DLI compute resources, which can be seen as a collection of DLI compute resources. DLI supports the creation of multiple queues within an elastic resource pool, and these queues can share the resources in the elastic resource pool.
-  **Queues** are the basic units of compute resources that are actually used and allocated in DLI. You can create different queues for different jobs or data processing tasks, and allocate and adjust resources for these queues as needed. To learn more about the queue types in DLI, refer to :ref:`DLI Queue Types <dli_01_0504__section391123318241>`.

.. note::

   DLI elastic resource pools are physically isolated, while queues within the same elastic resource pool are logically isolated.

   You are advised to create separate elastic resource pools for testing and production scenarios to ensure the independence and security of resource management through physical isolation.

DLI Compute Resource Modes
--------------------------

DLI offers three compute resource management modes, each with unique advantages and use cases.


.. figure:: /_static/images/en-us_image_0000002079726669.png
   :alt: **Figure 1** DLI compute resource modes

   **Figure 1** DLI compute resource modes

-  **Elastic resource pool mode**: a pooled management mode for compute resources that provides dynamic scaling capabilities. Queues within the same elastic resource pool share compute resources. By setting up a reasonable compute resource allocation policy for queues, you can improve compute resource utilization and meet resource demands during peak hours.

   -  Use cases: suitable for scenarios with significant fluctuations in business volume, such as periodic data batch processing tasks or real-time data processing needs.

   -  Supported queue types: for SQL (Spark), for SQL (HetuEngine), and for general purpose. To learn more about the queue types in DLI, refer to :ref:`DLI Queue Types <dli_01_0504__section391123318241>`.

      .. note::

         General-purpose queues and SQL queues in elastic resource pool mode do not support cross-AZ deployment.

   -  Usage: first create an elastic resource pool, then create queues within the pool and allocate compute resources. Queues are associated with specific jobs and data processing tasks.

      For how to buy an elastic resource pool and create queues within it, see :ref:`Creating an Elastic Resource Pool and Creating Queues Within It <dli_01_0505>`.

-  **Global sharing mode:**

   Global sharing mode is a compute resource allocation mode that allocates resources based on the actual amount of data scanned in SQL queries. It does not support specifying or reserving compute resources.

   The **default** queue, which is preset by DLI, is the compute resource for global sharing mode, and the resource size is allocated on demand. Users who are unsure of the data size or occasionally need to process data can use the **default** queue to run jobs.

   -  Use cases: suitable for testing jobs or scenarios with low resource consumption.

   -  Supported queue types: Only the preset **default** queue in DLI is the compute resource for global sharing mode.

      The **default** queue is typically used by users who are new to DLI but it may lead to resource contention and prevent you from getting the resources you need for your jobs, as its resources are shared among all users. You are advised to use self-built queues to run production jobs.

   -  Usage: The **default** queue is only applicable to submitting SQL jobs. When submitting SQL jobs on the DLI management console, select the **default** queue.

-  **Non-elastic resource pool mode (discarded and not recommended):**

   The previous-gen of DLI's compute resource management mode is no longer recommended due to its lack of flexibility.

   Non-elastic resource pool mode provides fixed-specification compute resources that are purchased and exclusively used, and cannot be dynamically adjusted according to demand, which may result in resource waste or insufficient resources during peak hours.

.. table:: **Table 1** DLI compute resource modes and supported queue types

   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DLI Compute Resource Mode          | Supported Queue Type | Resource Characteristic                                              | Use Case                                                                                                                                                 |
   +====================================+======================+======================================================================+==========================================================================================================================================================+
   | **Elastic resource pool mode**     | For SQL (Spark)      | Resources are shared among multiple queues for a single user.        | Suitable for scenarios with significant fluctuations in business demand, where resources need to be flexibly adjusted to meet peak and off-peak demands. |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   |                                    | For SQL (HetuEngine) | Resources are dynamically allocated and can be flexibly adjusted.    |                                                                                                                                                          |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   |                                    | For general purpose  |                                                                      |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Global sharing mode**            | default queue        | Resources are shared among multiple queues for multiple users.       | Suitable for temporary or testing projects where data size is uncertain or data processing is only required occasionally.                                |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   |                                    |                      | You are billed on a pay-per-use basis. Resources cannot be reserved. |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Non-elastic resource pool mode** | For SQL              | Resources are exclusively used by a single user and a single queue.  | Discarded and not recommended.                                                                                                                           |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   | **(discarded, not recommended)**   | For general purpose  | Resources cannot be dynamically adjusted and may remain idle.        |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

To help you understand the use cases for different DLI compute resource modes, we can compare purchasing DLI compute resources to using car services:

-  The elastic resource pool mode can be compared to "renting a car" where you can dynamically adjust the scale of resources based on actual needs.

   This mode is suitable for scenarios with significant fluctuations in business demand, allowing for flexible adjustment of resources based on peak and off-peak demands to optimize costs.

-  The global sharing mode can be compared to "taking a taxi" where you only pay for the actual amount of data used.

   This mode is suitable for scenarios where data size is uncertain or data processing is only required occasionally, allowing for on-demand use of resources without the need to pre-purchase or reserve resources.

Elastic Resource Pool Scaling
-----------------------------

Creating or deleting queues within an elastic resource pool triggers elastic resource scaling.

Scaling in an elastic resource pool may affect nodes containing shuffle data, leading to the recomputation of Spark tasks. This causes automatic retries for Spark and SQL jobs, and if the retries exceed the limit, the job execution fails, requiring you to rerun the job.

.. note::

   -  Spark 2.3 jobs need to be upgraded to a later Spark version to support dynamic scale-in of the jobs while they are running.
   -  Spark Streaming and Flink jobs cannot be scaled in while they are running. To perform a scale-in, suspend the jobs or migrate them to another elastic resource pool.

.. _dli_01_0504__section391123318241:

DLI Queue Types
---------------

DLI is divided into three queue types: **default** queue, for SQL, and for general purpose. You can choose the most suitable queue type based on your business scenario and job characteristics.

-  **default queue:**

   The **default** queue is a preset queue that is shared among all users.

   The **default** queue does not support specifying the size of resources and resources are allocated on-demand during job execution, with billing based on the actual amount of data scanned.

   As resources of the **default** queue are shared among all users, there may be resource contention during use, and it cannot be guaranteed that resources will be available for every job execution.

   The **default** queue is suitable for small-scale or temporary data processing needs. For important jobs or jobs that require guaranteed resources, you are advised to buy an elastic resource pool and create queues within it to execute jobs.

-  **For SQL:**

   For SQL queues are used to execute SQL jobs and supports specifying engine types including Spark and HetuEngine.

   This type of queues is suitable for businesses that require fast data query and analysis, as well as regular cache clearing or environment resetting.

-  **For general purpose:**

   For general purpose queues are used to execute Spark jobs, Flink OpenSource SQL jobs, and Flink Jar jobs.

   It is suitable for complex data processing, real-time data stream processing, or batch data processing scenarios.

Use Cases of Elastic Resource Pools
-----------------------------------

Queues in an elastic resource pool are recommended, as they offer the flexibility to use resources with high utilization as needed. This part describes common use cases of elastic resource pools.

**Resources too fixed to meet a range of requirements.**

The quantities of compute resources required for jobs change in different time of a day. If the resources cannot be scaled based on service requirements, they may be wasted or insufficient. :ref:`Figure 2 <dli_01_0504__fig6453203515012>` shows the resource usage during a day.

-  After ETL jobs are complete, no other jobs are running during 04:00 to 07:00 in the early morning. The resources could be released at that time.

-  From 09:00 to 12:00 a.m. and 02:00 to 04:00 p.m., a large number of ETL report and job queries are queuing for compute resources.

   .. _dli_01_0504__fig6453203515012:

   .. figure:: /_static/images/en-us_image_0000001309687485.png
      :alt: **Figure 2** Fixed resources

      **Figure 2** Fixed resources

**Resources are isolated and cannot be shared.**

A company has two departments, and each run their jobs on a DLI queue. Department A is idle from 08:00 to 12:00 a.m. and has remaining resources, while department B has a large number of service requests during this period and needs more resources to meet the requirements. Since the resources are isolated and cannot be shared between department A and B, the idle resources are wasted.


.. figure:: /_static/images/en-us_image_0000001309807469.png
   :alt: **Figure 3** Resource waste due to resource isolation

   **Figure 3** Resource waste due to resource isolation

Elastic resource pools can be accessed by different queues and automatically scaled to improve resource utilization and handle resource peaks.

You can use elastic resource pools to centrally manage and allocate resources. Multiple queues can be bound to an elastic resource pool to share the pooled resources.
