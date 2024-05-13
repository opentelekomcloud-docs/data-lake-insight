:original_name: dli_01_0516.html

.. _dli_01_0516:

Configuring Scaling Policies for Queues
=======================================

Scenario
--------

A company has multiple departments that perform data analysis in different periods during a day.

-  Department A requires a large number of compute resources from 00:00 a.m. to 09:00 a.m. In other time segments, only small tasks are running.
-  Department B requires a large number of compute resources from 10:00 a.m. to 10:00 p.m. Some periodical tasks are running in other time segments during a day.

In the preceding scenario, you can add two queues to an elastic resource pool: queue **test_a** for department A, and queue **test_b** for department B. You can add scaling policies for 00:00-09:00 and 10:00-23:00 respectively to the **test_a** and **test_b** queues. For jobs in other periods, you can modify the default scaling policy.

.. table:: **Table 1** Scaling policy

   +--------+----------------+----------+-----------------+-----------------------------------------+------------------+----------------+----------------------+
   | Queue  | Period         | Priority | CUs             | Default Period                          | Default Priority | Default CUs    | Remarks              |
   +========+================+==========+=================+=========================================+==================+================+======================+
   | test_a | [00:00, 09:00) | 20       | Minimum CU: 64  | The time segments beyond [00:00, 09:00) | 5                | Minimum CU: 16 | Jobs of department A |
   |        |                |          |                 |                                         |                  |                |                      |
   |        |                |          | Maximum CU: 128 |                                         |                  | Maximum CU: 32 |                      |
   +--------+----------------+----------+-----------------+-----------------------------------------+------------------+----------------+----------------------+
   | test_b | [10:00, 23:00) | 20       | Minimum CU: 64  | The time segments beyond [10:00, 23:00) | 5                | Minimum CU: 32 | Jobs of department B |
   |        |                |          |                 |                                         |                  |                |                      |
   |        |                |          | Maximum CU: 128 |                                         |                  | Maximum CU: 64 |                      |
   +--------+----------------+----------+-----------------+-----------------------------------------+------------------+----------------+----------------------+

Precautions
-----------

-  In any time segment of a day, the total minimum CUs of all queues in an elastic resource pool cannot be more than the minimum CUs of the pool.
-  In any time segment of a day, the maximum CUs of any queue in an elastic resource pool cannot be more than the maximum CUs of the pool.
-  The periods of scaling policies cannot overlap.
-  The period of a scaling policy can only be set by hour and specified by the start time and end time. For example, if you set the period to **00-09**, the period when the policy takes effect is [00:00, 09:00). The period of the default scaling policy cannot be modified.
-  In any period, compute resources are preferentially allocated to meet the minimum number of CUs of all queues. The remaining CUs (maximum CUs of the elastic resource pool - total minimum CUs of all queues) are allocated in accordance with the scaling policy priorities.

   -  A scaling policy with a smaller priority value (for example, 1) is prior to those with a bigger priority value (for example, 100).
   -  If the scaling policies of two queues have the same priority, resources are randomly allocated to a queue. If there are remaining resources, they are randomly allocated until there is no more left.

      .. table:: **Table 2** CU allocation

         +------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
         | Scenario                                                                                                               | CUs                                                                                                                                              |
         +========================================================================================================================+==================================================================================================================================================+
         | An elastic resource pool has a maximum number of 256 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                   |
         |                                                                                                                        |                                                                                                                                                  |
         | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 128                                            | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 160 CUs remaining.                     |
         | -  Queue B: priority 10; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                      | #. The remaining CUs are allocated based on the priorities. Queue B is prior to queue A. Therefore, queue B gets 64 CUs, and queue A has 96 CUs. |
         +------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
         | An elastic resource pool has a maximum number of 96 CUs for queue A and queue B. The scaling policies are as follows:  | From 00:00 a.m. to 09:00 a.m.:                                                                                                                   |
         |                                                                                                                        |                                                                                                                                                  |
         | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are no remaining CUs.                      |
         | -  Queue B: priority 10; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                      | #. The allocation is complete.                                                                                                                   |
         +------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
         | An elastic resource pool has a maximum number of 128 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                   |
         |                                                                                                                        |                                                                                                                                                  |
         | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 32 CUs remaining.                      |
         | -  Queue B: priority 10; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                      | #. The remaining 32 CUs are all preferentially allocated to queue B.                                                                             |
         +------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
         | An elastic resource pool has a maximum number of 128 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                   |
         |                                                                                                                        |                                                                                                                                                  |
         | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 32 CUs remaining.                      |
         | -  Queue B: priority 5; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                       | #. The two queues have the same priority, the remaining 32 CUs are randomly allocated to the two queues.                                         |
         +------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Setting a Scaling Policy
------------------------

#. Log in to the DLI management console and create an elastic resource pool. Set the minimum and maximum number of CUs of the pool to 128 and 256 respectively. For details, see :ref:`Creating an Elastic Resource Pool <dli_01_0505>`.

#. Choose **Resources** > **Resource Pool**. Locate the row that contains the created elastic resource pool, and click **Queue MGMT** in the **Operation** column.

#. Refer to :ref:`Adding a Queue <dli_01_0509>` to create the **test_a** queue and set the scaling policy.

   a. Set the priority of the default scaling policy to 5, **Min CU** to **16**, and **Max CU** to **32**.
   b. Click create to add a scaling policy. Set the priority to **20**, **Period** to **00--09**, **Min CU** to **64**, and **Max CU** to **128**.

#. View the scaling policy on the **Queue Management** page of the specific elastic resource pool.

   Click |image1| to view graphical statistics of priorities and CU settings for all time segments.

#. Refer to :ref:`Adding a Queue <dli_01_0509>` to create the **test_b** queue and set the scaling policy.

   a. Set the priority of the default scaling policy to **5**, **Min CU** to **32**, and **Max CU** to **64**.
   b. Click create to add a scaling policy. Set the priority to **20**, **Period** to **10--23**, **Min CU** to **64**, and **Max CU** to **128**.

#. View the scaling policy on the **Queue Management** page of the specific elastic resource pool.

   Click |image2| to view graphical statistics on priorities and CU settings of the two queues for all time segments.

.. |image1| image:: /_static/images/en-us_image_0000001309847549.png
.. |image2| image:: /_static/images/en-us_image_0000001262007480.png
