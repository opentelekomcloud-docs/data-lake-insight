:original_name: dli_01_0504.html

.. _dli_01_0504:

Overview
========

Elastic Resource Pool
---------------------

An elastic resource pool provides compute resources (CPU and memory) for running DLI jobs. The unit is CU. One CU contains one CPU and 4 GB memory.

You can create multiple queues in an elastic resource pool. Compute resources can be shared among queues. You can properly set the resource pool allocation policy for queues to improve compute resource utilization.

Specifications
--------------

DLI offers compute resources in the specifications listed in :ref:`Table 1 <dli_01_0504__table1791574931112>`.

.. _dli_01_0504__table1791574931112:

.. table:: **Table 1** Elastic resource pool specifications

   +-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Edition         | Specification    | Constraint                                                                                                                      | Scenario                                                                                                                                                                                                                |
   +=================+==================+=================================================================================================================================+=========================================================================================================================================================================================================================+
   | Basic           | 16-64 CUs        | -  High reliability and availability are not supported.                                                                         | This edition is suitable for testing scenarios with low resource consumption and low requirements for resource reliability and availability.                                                                            |
   |                 |                  | -  Queue properties and job priorities cannot be set.                                                                           |                                                                                                                                                                                                                         |
   |                 |                  | -  Notebook instances cannot be interconnected with.                                                                            |                                                                                                                                                                                                                         |
   |                 |                  |                                                                                                                                 |                                                                                                                                                                                                                         |
   |                 |                  | For more notes and constraints on elastic resource pools, see :ref:`Notes and Constraints <dli_01_0504__section1929612253596>`. |                                                                                                                                                                                                                         |
   +-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Standard        | 64 CUs or higher | For more notes and constraints on elastic resource pools, see :ref:`Notes and Constraints <dli_01_0504__section1929612253596>`. | This edition offers powerful computing capabilities, high availability, and flexible resource management. It is suitable for large-scale computing tasks and business scenarios with long-term resource planning needs. |
   +-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0504__section1929612253596:

Notes and Constraints
---------------------

-  The region of an elastic resource pool cannot be changed.

-  Jobs of Flink 1.10 or later can run in elastic resource pools.

-  The network segment of an elastic resource pool cannot be changed after being set.

-  You can only view the scaling history of resource pools in the last 30 days.

-  Elastic resource pools cannot access the Internet.

-  Changes to elastic resource pool CUs can occur when setting the CU, adding or deleting queues in an elastic resource pool, or modifying the scaling policies of queues in an elastic resource pool, or when the system automatically triggers elastic resource pool scaling. However, in some cases, the system cannot guarantee that the scaling will reach the target CUs as planned.

   -  If there are not enough physical resources, an elastic resource pool may not be able to scale out to the desired target size.

   -  The system does not guarantee that an elastic resource pool will be scaled in to the desired target size.

      The system checks the resource usage before scaling in the elastic resource pool to determine if there is enough space for scaling in. If the existing resources cannot be scaled in according to the minimum scaling step, the pool may not be scaled in successfully or only partially.

      The scaling step may vary depending on the resource specifications, usually 16 CUs, 32 CUs, 48 CUs, 64 CUs, etc.

      For example, if the elastic resource pool has a capacity of 192 CUs and the queues in the pool are using 68 CUs due to running jobs, the plan is to scale in to 64 CUs.

      When executing a scaling in task, the system determines that there are 124 CUs remaining and scales in by the minimum step of 64 CUs. However, the remaining 60 CUs cannot be scaled in any further. Therefore, after the elastic resource pool executes the scaling in task, its capacity is reduced to 128 CUs.

Scenario
--------

**Resources too fixed to meet a range of requirements.**

The quantities of compute resources required for jobs change in different time of a day. If the resources cannot be scaled based on service requirements, they may be wasted or insufficient. :ref:`Figure 1 <dli_01_0504__fig6453203515012>` shows the resource usage during a day.

-  After ETL jobs are complete, no other jobs are running during 04:00 to 07:00 in the early morning. The resources could be released at that time.

-  From 09:00 to 12:00 a.m. and 02:00 to 04:00 p.m., a large number of ETL report and job queries are queuing for compute resources.

   .. _dli_01_0504__fig6453203515012:

   .. figure:: /_static/images/en-us_image_0000001309687485.png
      :alt: **Figure 1** Fixed resources

      **Figure 1** Fixed resources

**Resources are isolated and cannot be shared.**

A company has two departments, and each run their jobs on a DLI queue. Department A is idle from 08:00 to 12:00 a.m. and has remaining resources, while department B has a large number of service requests during this period and needs more resources to meet the requirements. Since the resources are isolated and cannot be shared between department A and B, the idle resources are wasted.


.. figure:: /_static/images/en-us_image_0000001309807469.png
   :alt: **Figure 2** Resource waste due to resource isolation

   **Figure 2** Resource waste due to resource isolation

Elastic resource pools can be accessed by different queues and automatically scaled to improve resource utilization and handle resource peaks.

You can use elastic resource pools to centrally manage and allocate resources. Multiple queues can be bound to an elastic resource pool to share the pooled resources.

Architecture and Advantages
---------------------------

Elastic resource pools support the CCE cluster architecture for heterogeneous resources so you can centrally manage and allocate them.

Elastic resource pools have the following advantages:

-  **Unified management**

   -  You can manage multiple internal clusters and schedule jobs. You can manage millions of cores for compute resources.
   -  Elastic resource pools can be deployed across multiple AZs to support high availability. **(This function will be supported in later versions.)**

-  **Tenant resource isolation**

   Resources of different queues are isolated to reduce the impact on each other.

-  **Shared access and flexibility**

   -  Specifications can be scaled in seconds to help you handle request peaks.
   -  Queue priorities and CU quotas can be set at different time to improve resource utilization.

-  **Job-level isolation (supported in later versions)**

   SQL jobs can run on independent Spark instances, reducing mutual impacts between jobs.

-  **Automatic scaling (supported in later versions)**

   The queue quota is updated in real time based on workload and priority.

Using elastic resource pools has the following advantages.

+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Advantage             | No Elastic Resource Pool                                                                                                                                                  | Use Elastic Resource Pool                                                                                                                        |
+=======================+===========================================================================================================================================================================+==================================================================================================================================================+
| Scale-out duration    | You will need to spend several minutes manually scaling out.                                                                                                              | No manual intervention is required, as dynamic scale out can be done in seconds.                                                                 |
+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Resource utilization  | Resources cannot be shared among different queues.                                                                                                                        | Queues added to the same elastic resource pool can share compute resources.                                                                      |
|                       |                                                                                                                                                                           |                                                                                                                                                  |
|                       | For example, if queue 1 has 10 unused CUs and queue 2 requires more resources due to heavy load, queue 2 cannot utilize the resources of queue 1. It has to be scaled up. |                                                                                                                                                  |
+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
|                       | When you set a data source, you must allocate different network segments to each queue, which requires a large number of VPC network segments.                            | You can add multiple general-purpose queues in the same elastic resource pool to one network segment, simplifying the data source configuration. |
+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Resource allocation   | If resources are insufficient for scale-out tasks of multiple queues, some queues will fail to be scaled out.                                                             | You can set the priority for each queue in the elastic resource pool based on the peak hours to ensure proper resource allocation.               |
+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

You can perform the following operations on elastic resource pools:

-  :ref:`Creating an Elastic Resource Pool <dli_01_0505>`
-  :ref:`Managing Permissions <dli_01_0526>`
-  :ref:`Adding a Queue <dli_01_0509>`
-  :ref:`Binding a Queue <dli_01_0530>`
-  :ref:`Managing Queues <dli_01_0506>`
-  :ref:`Setting CUs <dli_01_0507>`
