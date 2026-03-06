:original_name: dli_01_0506.html

.. _dli_01_0506:

Adjusting Scaling Policies for Queues in an Elastic Resource Pool
=================================================================

Elastic resource pools support multiple queues to manage job execution efficiently. For details about how to create queues in a pool, see :ref:`Creating an Elastic Resource Pool and Creating Queues Within It <dli_01_0505>`. Once configured, you can define scaling policies based on each queue's compute resource usage patterns, such as peak and off-peak periods, and priority levels. This ensures optimal allocation of CUs to maintain stable and efficient job performance.

Precautions
-----------

-  You are advised to implement fine-grained management by separating Flink real-time streaming jobs from SQL batch processing jobs into distinct elastic resource pools. This approach offers two key benefits:

   Flink streaming jobs run continuously and require stability without forced scale-in, preventing interruptions and system instability.

   SQL batch jobs benefit from flexible scaling within their dedicated pool, significantly improving scaling success rates and operational efficiency.

-  At any time of day, the sum of minimum CUs across all queues in the elastic resource pool must not exceed the pool's minimum CUs.

-  Similarly, at any given time, no single queue's maximum CU allocation should exceed the pool's maximum CUs.

-  The time intervals for different scaling policies within the same queue must not overlap.

-  Scaling policy periods are set in whole-hour increments, including the start time but excluding the end time. For example, a 00-09 interval covers [00:00, 09:00). Note that default scaling policies do not allow modifications to these time settings.

-  During any period, the system first ensures all queues meet their minimum CU requirements. Any remaining CUs (calculated as the pool's maximum CUs minus the sum of all queues' minimum CUs) are allocated based on configured priorities until fully distributed.

-  Once a queue successfully scales out, billing begins for the additional CUs and continues until scale-in occurs. To avoid unnecessary costs, promptly release resources when they are no longer needed, as unused CUs will still incur charges.

   .. table:: **Table 1** CU allocation (without jobs)

      +------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Scenario                                                                                                               | CUs                                                                                                                                                                                                 |
      +========================================================================================================================+=====================================================================================================================================================================================================+
      | An elastic resource pool has a maximum number of 256 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                                                                      |
      |                                                                                                                        |                                                                                                                                                                                                     |
      | -  Queue A: priority 5; period: 00:00-09:00; minimum CUs: 32; maximum CUs: 64                                          | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 160 CUs remaining.                                                                        |
      | -  Queue B: priority 10; time period: 00:00-09:00; minimum CUs: 64; maximum CUs: 128                                   | #. The remaining CUs are allocated based on priority. Since queue B has a higher priority than queue A, 64 CUs will be allocated to queue B first, followed by the allocation of 32 CUs to queue A. |
      +------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | An elastic resource pool has a maximum number of 96 CUs for queue A and queue B. The scaling policies are as follows:  | From 00:00 a.m. to 09:00 a.m.:                                                                                                                                                                      |
      |                                                                                                                        |                                                                                                                                                                                                     |
      | -  Queue A: priority 5; period: 00:00-09:00; minimum CUs: 32; maximum CUs: 64                                          | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are no remaining CUs.                                                                         |
      | -  Queue B: priority 10; time period: 00:00-09:00; minimum CUs: 64; maximum CUs: 128                                   | #. The allocation is complete.                                                                                                                                                                      |
      +------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | An elastic resource pool has a maximum number of 128 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                                                                      |
      |                                                                                                                        |                                                                                                                                                                                                     |
      | -  Queue A: priority 5; period: 00:00-09:00; minimum CUs: 32; maximum CUs: 64                                          | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 32 CUs remaining.                                                                         |
      | -  Queue B: priority 10; time period: 00:00-09:00; minimum CUs: 64; maximum CUs: 128                                   | #. The remaining 32 CUs are preferentially allocated to queue B.                                                                                                                                    |
      +------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | An elastic resource pool has a maximum number of 128 CUs for queue A and queue B. The scaling policies are as follows: | From 00:00 a.m. to 09:00 a.m.:                                                                                                                                                                      |
      |                                                                                                                        |                                                                                                                                                                                                     |
      | -  Queue A: priority 5; period: 00:00-09:00; minimum CUs: 32; maximum CUs: 64                                          | #. The minimum CUs are allocated to the two queues. Queue A has 32 CUs, and queue B has 64 CUs. There are 32 CUs remaining.                                                                         |
      | -  Queue B: priority 5; time period: 00:00-09:00; minimum CUs: 64; maximum CUs: 128                                    | #. The two queues have the same priority, the remaining 32 CUs are randomly allocated to the two queues.                                                                                            |
      +------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** CU allocation (with jobs)

      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Scenario                                                                                    | Actual CUs of Elastic Resource Pool | CUs Allocated to Queue A | CUs Allocated to Queue B | Allocation Description                                                                                                                                                                      |
      +=============================================================================================+=====================================+==========================+==========================+=============================================================================================================================================================================================+
      | Queues A and B are added to the elastic resource pool. The scaling policies are as follows: | 192 CUs                             | 64 CUs                   | 128 CUs                  | If the actual CUs of the elastic resource pool are greater than or equal to the sum of the maximum CUs of the two queues,                                                                   |
      |                                                                                             |                                     |                          |                          |                                                                                                                                                                                             |
      | -  Queue A: period: 00:00-09:00; minimum CUs: 32; maximum CUs: 64                           |                                     |                          |                          | the maximum CUs are allocated to both queues.                                                                                                                                               |
      | -  Queue B: period: 00:00-09:00; minimum CUs: 64; maximum CUs: 128                          |                                     |                          |                          |                                                                                                                                                                                             |
      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      |                                                                                             | 96 CUs                              | 32 CUs                   | 64 CUs                   | The elastic resource pool preferentially meets the minimum CUs of the two queues.                                                                                                           |
      |                                                                                             |                                     |                          |                          |                                                                                                                                                                                             |
      |                                                                                             |                                     |                          |                          | After the minimum CUs are allocated to the two queues, no CUs are allocatable.                                                                                                              |
      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      |                                                                                             | 128 CUs                             | 32 CUs to 64 CUs         | 64 CUs to 96 CUs         | The elastic resource pool prioritizes meeting the minimum CUs of both queues, allocating 32 CUs to queue A and 64 CUs to queue B first, with an additional 32 CUs available for allocation. |
      |                                                                                             |                                     |                          |                          |                                                                                                                                                                                             |
      |                                                                                             |                                     |                          |                          | The remaining portion is distributed according to the queue's load and priority. The actual CUs of the queues fluctuate within the specified range.                                         |
      +---------------------------------------------------------------------------------------------+-------------------------------------+--------------------------+--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Managing Queues
---------------

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.
#. Locate the target elastic resource pool and click **Queue MGMT** in the **Operation** column. The **Queue Management** page is displayed.
#. View the queues added to the elastic resource pool.

   .. table:: **Table 3** Queue parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                             |
      +===================================+=========================================================================================================================================================+
      | Name                              | Name of the queue to add                                                                                                                                |
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

         -  **Priority**: The default value is **1**. The value ranges from 1 to 100. A larger value indicates a higher priority.
         -  **Period**:

            -  You can only set the period to hours in [start time,end time) format.
            -  For example, if you set **Period** to **01** and **17**, the scaling policy takes effect at 01:00 a.m. till 05:00 p.m.
            -  The periods of scaling policies with different priorities cannot overlap.

         -  **Max CUs** and **Min CUs**:

            -  In any time segment of a day, the total minimum CUs of all queues in an elastic resource pool cannot be more than the minimum CUs of the pool.
            -  In any time segment of a day, the maximum CUs of any queue in an elastic resource pool cannot be more than the maximum CUs of the pool.

#. After you finish the settings, click statistics icon in the upper right corner of the queue list to view all scaling policies of all queue in the elastic resource pool.
#. View the scaling task generated when the scaling starts. Go to **Job Management** > **SQL Jobs** and view the jobs of the **SCALE_QUEUE** type.
