:original_name: dli_01_0504.html

.. _dli_01_0504:

Overview of DLI Elastic Resource Pools and Queues
=================================================

DLI compute resources are the foundation to run jobs. This section describes the modes of DLI compute resources and queue types.

What Are Elastic Resource Pools and Queues?
-------------------------------------------

Before we dive into the compute resource modes of DLI, let us first understand the basic concepts of elastic resource pools and queues.

-  **Elastic resource pool**

   An elastic resource pool is a pooled management mode for DLI compute resources, which can be seen as a collection of DLI compute resources. DLI supports the creation of multiple queues within an elastic resource pool, and these queues can share the resources in the elastic resource pool.

   For details about the product specifications of elastic resource pools, see :ref:`Elastic Resource Pool Specifications <dli_01_0504__section129548131110>`.

   For more about the advantages of elastic resource pools, see :ref:`Advantages of Elastic Resource Pools <dli_01_0504__section20762189174817>`.

   -  The physical resource layer of an elastic resource pool consists of compute nodes distributed in different AZs, supporting cross-AZ HA.

   -  Multiple queues within the same resource pool share physical resources but maintain logical isolation to enforce resource allocation policies such as priorities and quotas.

   -  Elastic resource pools can adjust resources in real time based on queue loads, achieving on-demand auto scaling in minutes.

   -  An elastic resource pool can simultaneously support SQL, Spark, and Flink jobs. The specific job types supported depend on the queue types created within the elastic resource pool.

      Refer to :ref:`DLI Compute Resource Modes and Supported Queue Types <dli_01_0504__section1666431292413>`.

-  .. _dli_01_0504__li1290916414444:

   **Queue**

   Queues are the basic units of compute resources that are actually used and allocated in DLI. You can create different queues for different jobs or data processing tasks, and allocate and adjust resources for these queues as needed.

   DLI is divided into three queue types: **default** queue, for SQL, and for general purpose. You can choose the most suitable queue type based on your business scenario and job characteristics.

   -  **default queue:**

      The **default** queue is a preset queue that is shared among all users.

      The **default** queue does not support specifying the size of resources and resources are allocated on-demand during job execution, with billing based on the actual amount of data scanned.

      As resources of the **default** queue are shared among all users, there may be resource contention during use, and it cannot be guaranteed that resources will be available for every job execution.

      The **default** queue is suitable for small-scale or temporary data processing needs. For important jobs or jobs that require guaranteed resources, you are advised to buy an elastic resource pool and create queues within it to execute jobs.

   -  **For SQL:**

      For SQL queues are designed to execute SQL jobs. They support the Spark engine.

      This type of queues is suitable for businesses that require fast data query and analysis, as well as regular cache clearing or environment resetting.

   -  **For general purpose:**

      For general purpose queues are used to execute Spark jobs, Flink OpenSource SQL jobs, and Flink Jar jobs.

      It is suitable for complex data processing, real-time data stream processing, or batch data processing scenarios.

.. note::

   DLI elastic resource pools are physically isolated, while queues within the same elastic resource pool are logically isolated.

   You are advised to create separate elastic resource pools for testing and production scenarios to ensure the independence and security of resource management through physical isolation.

DLI Compute Resource Modes
--------------------------

DLI offers three compute resource management modes, each with unique advantages and use cases.


.. figure:: /_static/images/en-us_image_0000002079726669.png
   :alt: **Figure 1** DLI compute resource modes

   **Figure 1** DLI compute resource modes

-  **Elastic resource pool mode:**

   A pooled management mode for compute resources that provides dynamic scaling capabilities. Queues within the same elastic resource pool share compute resources. By setting up a reasonable compute resource allocation policy for queues, you can improve compute resource utilization and meet resource demands during peak hours.

   -  Use cases: suitable for scenarios with significant fluctuations in business volume, such as periodic data batch processing tasks or real-time data processing needs.

   -  Supported queue types: for SQL (Spark) and for general purpose. For details about DLI queue types, see :ref:`Queue Types <dli_01_0504__li1290916414444>`.

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

-  **Non-elastic resource pool mode (deprecated and not recommended):**

   The previous-gen of DLI's compute resource management mode is no longer recommended due to its lack of flexibility.

   Non-elastic resource pool mode provides fixed-specification compute resources that are purchased and exclusively used, and cannot be dynamically adjusted according to demand, which may result in resource waste or insufficient resources during peak hours.

To help you understand the use cases for different DLI compute resource modes, we can compare purchasing DLI compute resources to using car services:

-  The elastic resource pool mode can be compared to "renting a car" where you can dynamically adjust the scale of resources based on actual needs.

   This mode is suitable for scenarios with significant fluctuations in business demand, allowing for flexible adjustment of resources based on peak and off-peak demands to optimize costs.

-  The global sharing mode can be compared to "taking a taxi" where you only pay for the actual amount of data used.

   This mode is suitable for scenarios where data size is uncertain or data processing is only required occasionally, allowing for on-demand use of resources without the need to pre-purchase or reserve resources.

.. _dli_01_0504__section1666431292413:

DLI Compute Resource Modes and Supported Queue Types
----------------------------------------------------

:ref:`Table 1 <dli_01_0504__table2051811102459>` describes the queue types supported by different compute resources in DLI.

.. _dli_01_0504__table2051811102459:

.. table:: **Table 1** DLI compute resource modes and supported queue types

   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DLI Compute Resource Mode          | Supported Queue Type | Resource Characteristic                                              | Use Case                                                                                                                                                 |
   +====================================+======================+======================================================================+==========================================================================================================================================================+
   | **Elastic resource pool mode**     | For SQL (Spark)      | Resources are shared among multiple queues for a single user.        | Suitable for scenarios with significant fluctuations in business demand, where resources need to be flexibly adjusted to meet peak and off-peak demands. |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   |                                    | For general purpose  | Resources are dynamically allocated and can be flexibly adjusted.    |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Global sharing mode**            | default queue        | Resources are shared among multiple queues for multiple users.       | Suitable for temporary or testing projects where data size is uncertain or data processing is only required occasionally.                                |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   |                                    |                      | You are billed on a pay-per-use basis. Resources cannot be reserved. |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Non-elastic resource pool mode** | For SQL              | Resources are exclusively used by a single user and a single queue.  | Deprecated, not recommended                                                                                                                              |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   | **(deprecated, not recommended)**  | For general purpose  | Resources cannot be dynamically adjusted and may remain idle.        |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0504__section129548131110:

Elastic Resource Pool Specifications
------------------------------------

An elastic resource pool provides compute resources (CPU and memory) for running DLI jobs. The unit is CU. One CU contains 1 vCPU and 4 GB memory.

You can create multiple queues in an elastic resource pool. Compute resources can be shared among queues. By appropriately setting up compute resource allocation policies for queues, you can enhance compute resource utilization.

.. note::

   DLI elastic resource pools are physically isolated, while queues within the same elastic resource pool are logically isolated.

   You are advised to create separate elastic resource pools for testing and production scenarios to ensure the independence and security of resource management through physical isolation.

:ref:`Table 2 <dli_01_0504__dli_07_0027_table1791574931112>` lists the specifications of the elastic resource pools provided by DLI.

.. _dli_01_0504__dli_07_0027_table1791574931112:

.. table:: **Table 2** Elastic resource pool specifications

   +-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Edition         | Specifications   | Notes and Constraints                                                                                                                          | Use Case                                                                                                                                                                                                                |
   +=================+==================+================================================================================================================================================+=========================================================================================================================================================================================================================+
   | Basic           | 16-64 CUs        | -  High reliability and availability are not supported.                                                                                        | This edition is suitable for testing scenarios with low resource consumption and low requirements for resource reliability and availability.                                                                            |
   |                 |                  | -  Queue properties cannot be set.                                                                                                             |                                                                                                                                                                                                                         |
   |                 |                  | -  Job priorities are not supported.                                                                                                           |                                                                                                                                                                                                                         |
   |                 |                  | -  Notebook instances cannot be interconnected with.                                                                                           |                                                                                                                                                                                                                         |
   +-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Standard        | 64 CUs or higher | For notes and constraints on using an elastic resource pool, see :ref:`Notes and Constraints on Using an Elastic Resource Pool <dli_01_0505>`. | This edition offers powerful computing capabilities, high availability, and flexible resource management. It is suitable for large-scale computing tasks and business scenarios with long-term resource planning needs. |
   +-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0504__section20762189174817:

Advantages of Elastic Resource Pools
------------------------------------

Elastic resource pools have the following advantages:

-  **Unified resource management**

   You can manage multiple internal clusters and schedule jobs in a unified manner. The scale of compute resources can reach million vCPUs.

-  **Tenant resource isolation**

   Resources of different queues are isolated to reduce the impact on each other.

-  **Time-based on-demand elasticity**

   -  Minute-level scaling to cope with traffic peaks and resource requirements.
   -  You can queue priorities and CU quotas at different times to improve resource utilization.

-  **Job-level resource isolation (not implemented currently and will be supported in later versions)**

   You can run SQL jobs on independent Spark instances, reducing mutual impacts between jobs.

-  **Automatic scaling (not implemented currently and will be supported in later versions)**

   Queue quotas are automatically updated in real time based on queue loads and priorities.

The advantages of using elastic resource pools include:

.. table:: **Table 3** Advantages of elastic resource pools

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Dimension             | No Elastic Resource Pool                                                                                                                                                  | Elastic Resource Pool                                                                                                                                                     |
   +=======================+===========================================================================================================================================================================+===========================================================================================================================================================================+
   | Expansion duration    | You will need to spend several minutes manually scaling out.                                                                                                              | No manual intervention is required, as dynamic scale out can be done in seconds.                                                                                          |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Resource utilization  | Resources cannot be shared among queues.                                                                                                                                  | Multiple queues added to the same elastic resource pool can share CU resources, enhancing resource utilization.                                                           |
   |                       |                                                                                                                                                                           |                                                                                                                                                                           |
   |                       | For example, if queue 1 has 10 unused CUs and queue 2 requires more resources due to heavy load, queue 2 cannot utilize the resources of queue 1. It has to be scaled up. |                                                                                                                                                                           |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | When creating a datasource connection, you need to assign non-overlapping network segments to each queue, consuming a significant amount of VPC network segments.         | You can centrally assign network segments to multiple queues in an elastic resource pool, thereby simplifying datasource configuration.                                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Resource allocation   | You cannot set priorities when scaling out multiple queues concurrently. If there are insufficient resources, some queues will fail to be scaled out.                     | You can set the priority for each queue in an elastic resource pool based on the peak and off-peak hours of the current service to ensure reasonable resource allocation. |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Use Cases of Elastic Resource Pools
-----------------------------------

Queues in an elastic resource pool are recommended, as they offer the flexibility to use resources with high utilization as needed. This part describes common use cases of elastic resource pools.

**Resources too fixed to meet a range of requirements.**

The quantities of compute resources required for jobs change in different time of a day. If the resources cannot be scaled based on service requirements, they may be wasted or insufficient. :ref:`Figure 2 <dli_01_0504__fig6453203515012>` shows the resource usage during a day.

-  During the data period from approximately 04:00 to 07:00 in the morning, there are no other jobs running after ETL jobs complete. Since the resources remain constantly occupied, it leads to significant resource wastage.

-  Between 09:00 a.m. to 12:00 p.m. and 14:00 to 16:00, the volume of ETL report and job query requests is high. Due to insufficient allocated resources, jobs end up queuing continuously.

   .. _dli_01_0504__fig6453203515012:

   .. figure:: /_static/images/en-us_image_0000001309687485.png
      :alt: **Figure 2** Fixed resources

      **Figure 2** Fixed resources

**Resources are isolated and cannot be shared.**

A company has two departments with different jobs running on two queues of DLI. Department A experiences low activity from 8:00 a.m. to 12:00 p.m., leaving some resources unused, while department B faces high demand during this time, exceeding its current resource specifications. When scaling up is necessary, it cannot utilize the idle queue resources of department A, leading to resource wastage.


.. figure:: /_static/images/en-us_image_0000001309807469.png
   :alt: **Figure 3** Resource waste due to resource isolation

   **Figure 3** Resource waste due to resource isolation

Elastic resource pools can be accessed by different queues and automatically scaled to improve resource utilization and handle resource peaks.

You can use elastic resource pools to centrally manage and allocate resources. Multiple queues can be bound to an elastic resource pool to share the pooled resources.
