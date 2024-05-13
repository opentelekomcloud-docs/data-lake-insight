:original_name: dli_01_0506.html

.. _dli_01_0506:

Managing Queues
===============

Multiple queues can be added to an elastic resource pool. For details about how to add a queue, see :ref:`Adding a Queue <dli_01_0509>`. You can configure the number of CUs you want based on the compute resources used by DLI queues during peaks and troughs and set priorities for the scaling policies to ensure stable running of jobs.

Precautions
-----------

-  In any time segment of a day, the total minimum CUs of all queues in an elastic resource pool cannot be more than the minimum CUs of the pool.
-  In any time segment of a day, the maximum CUs of any queue in an elastic resource pool cannot be more than the maximum CUs of the pool.
-  The periods of scaling policies cannot overlap.
-  The period of a scaling policy can only be set by hour and specified by the start time and end time. For example, if you set the period to **00-09**, the time range when the policy takes effect is [00:00, 09:00). The period of the default scaling policy cannot be modified.
-  In any period, compute resources are preferentially allocated to meet the minimum number of CUs of all queues. The remaining CUs (total CUs of the elastic resource pool - total minimum CUs of all queues) are allocated in accordance with the scaling policy priorities.
-  After the queue is scaled out, the system starts billing you for the added CUs. So, if you do not have sufficient requirements, scale in your queue to release unnecessary CUs to save cost.

   .. table:: **Table 1** CU allocation (without jobs)

      +------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Scenario                                                                                                               | CUs                                                                                                                                               |
      +========================================================================================================================+===================================================================================================================================================+
      | An elastic resource pool has a maximum number of 256 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                    |
      |                                                                                                                        |                                                                                                                                                   |
      | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 160 CUs remaining.                      |
      | -  Queue B: priority 10; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                      | #. The remaining CUs are allocated based on the priorities. Queue B is prior to queue A. Therefore, queue B gets 128 CUs, and queue A has 32 CUs. |
      +------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | An elastic resource pool has a maximum number of 96 CUs for queue A and queue B. The scaling policies are as follows:  | From 00:00 a.m. to 09:00 a.m.:                                                                                                                    |
      |                                                                                                                        |                                                                                                                                                   |
      | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are no remaining CUs.                       |
      | -  Queue B: priority 10; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                      | #. The allocation is complete.                                                                                                                    |
      +------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | An elastic resource pool has a maximum number of 128 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                    |
      |                                                                                                                        |                                                                                                                                                   |
      | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 32 CUs remaining.                       |
      | -  Queue B: priority 10; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                      | #. The remaining 32 CUs are preferentially allocated to queue B.                                                                                  |
      +------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | An elastic resource pool has a maximum number of 128 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                    |
      |                                                                                                                        |                                                                                                                                                   |
      | -  Queue A: priority 5; period: 00:00-9:00; minimum CU: 32; maximum CU: 64                                             | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 32 CUs remaining.                       |
      | -  Queue B: priority 5; time period: 00:00-9:00; minimum CU: 64; maximum CU: 128                                       | #. The two queues have the same priority, the remaining 32 CUs are randomly allocated to the two queues.                                          |
      +------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** CU allocation (with jobs)

      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Scenario                                                                                    | Actual CUs of Elastic Resource Pool | CUs Allocated to Queue A | CUs Allocated to Queue B | Allocation Description                                                                                                                                                                               |
      +=============================================================================================+=====================================+==========================+==========================+======================================================================================================================================================================================================+
      | Queues A and B are added to the elastic resource pool. The scaling policies are as follows: | 192 CUs                             | 64 CUs                   | 128 CUs                  | If the actual CUs of the elastic resource pool are greater than or equal to the sum of the maximum CUs of the two queues,                                                                            |
      |                                                                                             |                                     |                          |                          |                                                                                                                                                                                                      |
      | -  Queue A: period: 00:00-9:00; minimum CU: 32; maximum CU: 64                              |                                     |                          |                          | the maximum CUs are allocated to both queues.                                                                                                                                                        |
      | -  Queue B: period: 00:00-9:00; minimum CU: 64; maximum CU: 128                             |                                     |                          |                          |                                                                                                                                                                                                      |
      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      |                                                                                             | 96 CUs                              | 32 CUs                   | 64 CUs                   | The elastic resource pool preferentially meets the minimum CUs of the two queues.                                                                                                                    |
      |                                                                                             |                                     |                          |                          |                                                                                                                                                                                                      |
      |                                                                                             |                                     |                          |                          | After the minimum CUs are allocated to the two queues, no CUs are allocatable.                                                                                                                       |
      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      |                                                                                             | 128 CUs                             | 32 CUs to 64 CUs         | 64 CUs to 96 CUs         | The elastic resource pool preferentially meets the minimum CUs of the two queues. That is, 32 CUs are allocated to queue A, 64 CUs are allocated to queue B, and the remaining 32 CUs are available. |
      |                                                                                             |                                     |                          |                          |                                                                                                                                                                                                      |
      |                                                                                             |                                     |                          |                          | The remaining CUs are allocated based on the queue load and priority. The actual CUs of the queue change within the range listed.                                                                    |
      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


Managing Queues
---------------

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.
#. Locate the target elastic resource pool and click **Queue MGMT** in the **Operation** column. The **Queue Management** page is displayed.
#. View the queues added to the elastic resource pool.

   .. table:: **Table 3** Queue parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                             |
      +===================================+=========================================================================================================================================================+
      | Name                              | Queue name                                                                                                                                              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type                              | Queue type                                                                                                                                              |
      |                                   |                                                                                                                                                         |
      |                                   | -  For SQL                                                                                                                                              |
      |                                   | -  For general purpose                                                                                                                                  |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Period                            | The start and end time of the queue scaling policy. This time range includes the start time but not the end time, that is, [start time, end time).      |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Min CUs                           | Minimum number of CUs allowed by the scaling policy.                                                                                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Max CUs                           | Maximum number of CUs allowed by the scaling policy.                                                                                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Priority                          | Priority of the scaling policy for a queue in the elastic resource pool. The priority ranges from 1 to 100. A smaller value indicates a lower priority. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Engine                            | For a queue running SQL jobs, the engine is Spark.                                                                                                      |
      |                                   |                                                                                                                                                         |
      |                                   | For a queue for general purpose, the engine can be Spark or Flink, but it is displayed by -- in this page.                                              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Created                           | Time when a queue is added to the elastic resource pool                                                                                                 |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Enterprise Project                | Enterprise project the queue belongs to.                                                                                                                |
      |                                   |                                                                                                                                                         |
      |                                   | Queues under different enterprise projects can be added to an elastic resource pool.                                                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Owner                             | User who added this queue                                                                                                                               |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operation                         | -  Edit: Modify or add a scaling policy.                                                                                                                |
      |                                   | -  Delete: Delete the queue.                                                                                                                            |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Locate the target queue and click **Edit** in the **Operation** column.
#. In the displayed **Queue Management** pane, perform the following operations as needed:

   -  **Add**: Click **Create** to add a scaling policy. Set **Priority**, **Period**, **Min CU**, and **Max CU**, and click **OK**.
   -  **Modify**: Modify parameters of an existing scaling policy and click **OK**.
   -  **Delete**: Locate the row that contains the scaling policy you want, click **Delete** and click **OK**.

      .. note::

         The **Priority** and **Period** parameters must meet the following requirements:

         -  **Priority**: The default value is **1**. The value ranges from 1 to 100. A bigger value indicates a higher priority.
         -  **Period**:

            -  You can only set the period to hours in [start time,end time) format.
            -  For example, if the **Period** to **01** and **17**, the scaling policy takes effect at 01:00 a.m. till 05:00 p.m.
            -  The periods of scaling policies with different priorities cannot overlap.

         -  **Max CUs** and **Min CUs**:

            -  In any time segment of a day, the total minimum CUs of all queues in an elastic resource pool cannot be more than the minimum CUs of the pool.
            -  In any time segment of a day, the maximum CUs of any queue in an elastic resource pool cannot be more than the maximum CUs of the pool.

#. After you finish the settings, click statistics icon in the upper right corner of the queue list to view all scaling policies of all queue in the elastic resource pool.
#. View the scaling task generated when the scaling starts. Go to **Job Management** > **SQL Jobs** and view the jobs of the **SCALE_QUEUE** type.
