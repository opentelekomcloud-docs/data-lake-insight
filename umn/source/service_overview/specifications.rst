:original_name: dli_07_0027.html

.. _dli_07_0027:

Specifications
==============

Elastic Resource Pool Specifications
------------------------------------

An elastic resource pool provides compute resources (CPU and memory) for running DLI jobs. The unit is CU. One CU contains one CPU and 4 GB memory.

You can create multiple queues in an elastic resource pool. Compute resources can be shared among queues. You can properly set the resource pool allocation policy for queues to improve compute resource utilization.

.. note::

   DLI elastic resource pools are physically isolated, while queues within the same elastic resource pool are logically isolated.

   You are advised to create separate elastic resource pools for testing and production scenarios to ensure the independence and security of resource management through physical isolation.

:ref:`Table 1 <dli_07_0027__table1791574931112>` lists the specifications of the elastic resource pools provided by DLI.

.. _dli_07_0027__table1791574931112:

.. table:: **Table 1** Elastic resource pool specifications

   +-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Edition         | Specifications   | Notes and Constraints                                                                                                                          | Use Case                                                                                                                                                                                                                |
   +=================+==================+================================================================================================================================================+=========================================================================================================================================================================================================================+
   | Basic           | 16-64 CUs        | -  High reliability and availability are not supported.                                                                                        | This edition is suitable for testing scenarios with low resource consumption and low requirements for resource reliability and availability.                                                                            |
   |                 |                  | -  Queue properties and job priorities cannot be set.                                                                                          |                                                                                                                                                                                                                         |
   |                 |                  | -  Notebook instances cannot be interconnected with.                                                                                           |                                                                                                                                                                                                                         |
   +-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Standard        | 64 CUs or higher | For notes and constraints on using an elastic resource pool, see :ref:`Notes and Constraints on Using an Elastic Resource Pool <dli_01_0505>`. | This edition offers powerful computing capabilities, high availability, and flexible resource management. It is suitable for large-scale computing tasks and business scenarios with long-term resource planning needs. |
   +-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
