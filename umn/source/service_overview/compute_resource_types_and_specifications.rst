:original_name: dli_07_0027.html

.. _dli_07_0027:

Compute Resource Types and Specifications
=========================================

DLI compute resources are the foundation for job execution. Both DLI's elastic resource pools and queues fall under compute resources. This section introduces the types and specifications of DLI compute resources.

What Are Elastic Resource Pools and Queues?
-------------------------------------------

Before we dive into the compute resource modes of DLI, let us first understand the basic concepts of elastic resource pools and queues.

-  **Elastic resource pool**

   An **elastic resource pool** is a pooled management mode for DLI compute resources, which can be seen as a collection of DLI compute resources. DLI supports the creation of multiple queues within an elastic resource pool, and these queues can share the resources in the elastic resource pool.

   For details about the specifications of elastic resource pools, see :ref:`Elastic Resource Pool Specifications <dli_07_0027__section2030115259109>`.

   -  The physical resource layer of an elastic resource pool consists of compute nodes distributed in different AZs.

   -  Multiple queues within the same resource pool share physical resources but maintain logical isolation to enforce resource allocation policies such as priorities and quotas.

   -  Elastic resource pools can adjust resources in real time based on queue loads, achieving on-demand auto scaling in minutes.

   -  An elastic resource pool can simultaneously support SQL, Spark, and Flink jobs. The specific job types supported depend on the queue types created within the elastic resource pool.

      Refer to :ref:`DLI Compute Resource Modes and Supported Queue Types <dli_07_0027__section1666431292413>`.

-  .. _dli_07_0027__li1752524818338:

   **Queue**

   Queues are the basic units of compute resources that are actually used and allocated in DLI. You can create different queues for different jobs or data processing tasks, and allocate and adjust resources for these queues as needed.

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

DLI Compute Resource Modes
--------------------------

DLI offers three compute resource management modes, each with unique advantages and use cases.


.. figure:: /_static/images/en-us_image_0000002398199205.png
   :alt: **Figure 1** DLI compute resource modes

   **Figure 1** DLI compute resource modes

-  **Elastic resource pool mode:**

   A pooled management mode for compute resources that provides dynamic scaling capabilities. Queues within the same elastic resource pool share compute resources. By appropriately setting up compute resource allocation policies for queues, you can enhance compute resource utilization and handle peak business demands efficiently.

   -  Use cases: suitable for scenarios with significant fluctuations in business volume, such as periodic data batch processing tasks or real-time data processing needs.
   -  Supported queue types: for SQL (Spark), for SQL (HetuEngine), and for general purpose. For details about DLI queue types, see :ref:`Queue Types <dli_07_0027__li1752524818338>`.

      .. note::

         General-purpose queues and SQL queues in elastic resource pool mode do not support cross-AZ deployment.

   -  Usage: first create an elastic resource pool, then create queues within the pool and allocate compute resources. Queues are associated with specific jobs and data processing tasks.

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

.. _dli_07_0027__section1666431292413:

DLI Compute Resource Modes and Supported Queue Types
----------------------------------------------------

:ref:`Table 1 <dli_07_0027__table25275211495>` describes the queue types supported by different compute resources in DLI.

.. _dli_07_0027__table25275211495:

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
   | **Non-elastic resource pool mode** | For SQL              | Resources are exclusively used by a single user and a single queue.  | Deprecated, not recommended                                                                                                                              |
   |                                    |                      |                                                                      |                                                                                                                                                          |
   | **(deprecated, not recommended)**  | For general purpose  | Resources cannot be dynamically adjusted and may remain idle.        |                                                                                                                                                          |
   +------------------------------------+----------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_07_0027__section2030115259109:

Elastic Resource Pool Specifications
------------------------------------

An elastic resource pool provides compute resources (CPU and memory) for running DLI jobs. The unit is CU. One CU contains 1 vCPU and 4 GB memory.

You can create multiple queues in an elastic resource pool. Compute resources can be shared among queues. By appropriately setting up compute resource allocation policies for queues, you can enhance compute resource utilization.

.. note::

   DLI elastic resource pools are physically isolated, while queues within the same elastic resource pool are logically isolated.

   You are advised to create separate elastic resource pools for testing and production scenarios to ensure the independence and security of resource management through physical isolation.

:ref:`Table 2 <dli_07_0027__table1791574931112>` lists the specifications of the elastic resource pools provided by DLI.

.. _dli_07_0027__table1791574931112:

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
