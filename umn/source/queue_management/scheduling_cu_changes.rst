:original_name: dli_01_0488.html

.. _dli_01_0488:

Scheduling CU Changes
=====================

Scenario
--------

When services are busy, you might need to use more compute resources to process services in a period. After this period, you do not require the same amount of resources. If the purchased queue specifications are small, resources may be insufficient during peak hours. If the queue specifications are large, resources may be wasted.

DLI provides scheduled tasks for elastic scale-in and -out in the preceding scenario. You can set different queue sizes (CUs) at different time or in different periods based on your service period or usage and the existing queue specifications to meet your service requirements and reduce costs.

Precautions
-----------

-  Periodic scaling can be performed for a newly created queue only when there were jobs running in this queue.
-  Scheduled scaling tasks are available only for a queue with more than 64 CUs. That is, the minimum specifications of a queue are 64 CUs.
-  A maximum of 12 scheduled tasks can be created for each queue.
-  When each scheduled task starts, the actual start time of the specification change has a deviation of 5 minutes. It is recommended that the task start time be at least 20 minutes earlier than the time when the queue is actually used.
-  The interval between two scheduled tasks must be at least 2 hours.
-  Changing the specifications of a queue is time-consuming. The time required for changing the specifications depends on the difference between the target specifications and the current specifications. You can view the specifications of the current queue on the **Queue Management** page.
-  If a job is running in the current queue, the queue may fail to be scaled in to the target CU amount value. Instead, it will be scaled in to a value between the current queue specifications and the target specifications. The system will try to scale in again 1 hour later until the next scheduled task starts.
-  If a scheduled task does not scale out or scale in to the target CU amount value, the system triggers the scaling plan again 15 minutes later until the next scheduled task starts.

Creating Periodic Task
----------------------

-  If only scale-out or scale-in is required, you need to create only one task for changing specifications. Set the **Task Name**, **Final CU Count**, and **Executed** parameters. For details, see :ref:`Table 1 <dli_01_0488__table828194717819>`.
-  To set both scale-out and scale-in parameters, you need to create two periodic tasks, and set the **Task Name**, **Final CU Count**, and **Executed** parameters. For details, see :ref:`Table 1 <dli_01_0488__table828194717819>`.

The procedure is as follows:

#. On the left of the DLI management console, click **Resources** > **Queue Management**.

#. Locate the queue for which you want to schedule a periodic task for elastic scaling, and choose **More** > **Schedule CU Changes** in the **Operation** column.

#. On the displayed page, click **Create Periodic Task** in the upper right corner.

#. On the **Create Periodic Task** page, set the required parameters. Click **OK**.

   .. _dli_01_0488__table828194717819:

   .. table:: **Table 1** Parameter description

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                  |
      +===================================+==============================================================================================================================================================================================================================================+
      | Task Name                         | Enter the name of the periodic task.                                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   | -  The task name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_) or be left unspecified.                                                                               |
      |                                   | -  The name can contain a maximum of 128 characters.                                                                                                                                                                                         |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Enable Task                       | Whether to enable periodic elastic scaling. The task is enabled by default. If disabled, the task will not be triggered on time.                                                                                                             |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Validity Period                   | Time segment for executing the periodic task. The options include **Date** and **Time**.                                                                                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Actual CUs                        | Queue specifications before scale-in or scale-out.                                                                                                                                                                                           |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Final CUs                         | Specifications after the queue is scaled in or out.                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   | .. note::                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   |    -  By default, the maximum specifications of a queue are **512 CUs**.                                                                                                                                                                     |
      |                                   |    -  The minimum queue specifications for scheduled scaling are 64 CUs. That is, only when **Actual CUs** are more than 64 CUs, the scheduled scaling can be performed.                                                                     |
      |                                   |    -  The value of **Actual CUs** must be a multiple of 16.                                                                                                                                                                                  |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Repeat                            | Time when scheduled scale-out or scale-in is repeat. Scheduled tasks can be scheduled by week in **Repeat**.                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   | -  By default, this parameter is not configured, indicating that the task is executed only once at the time specified by **Executed**.                                                                                                       |
      |                                   | -  If you select all, the plan is executed every day.                                                                                                                                                                                        |
      |                                   | -  If you select some options of **Repeat**, the plan is executed once a week at all specified days.                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   | .. note::                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   |    -  You do not need to set this parameter if you only need to perform scale-in or scale-out once.                                                                                                                                          |
      |                                   |    -  If you have set scaling, you can set **Repeat** as required. You can also set the repeat period together with the validity period.                                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | **Executed**                      | Time when scheduled scale-out or scale-in is performed                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                              |
      |                                   | -  When each scheduled task starts, the actual start time of the specification change has a deviation of 5 minutes. It is recommended that the task start time be at least 20 minutes earlier than the time when the queue is actually used. |
      |                                   | -  The interval between two scheduled tasks must be at least 2 hours.                                                                                                                                                                        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   After a periodic task is created, you can view the specification change of the current queue and the latest execution time on the page for scheduling CU changes.

   Alternatively, on the **Queue Management** page, check whether the **Specifications** change to determine whether the scaling is successful.

   You can also go to the **Job Management** page and check the status of the **SCALE_QUEUE** job. If the job status is **Scaling**, the queue is being scaled in or out.

Modifying a Scheduled Task
--------------------------

If a periodic task cannot meet service requirements anymore, you can modify it on the **Schedule CU Changes** page.

#. In the navigation pane of the DLI management console, choose **Resources** >\ **Queue Management**.
#. Locate the queue for which you want to schedule a periodic task for elastic scaling, and choose **More** > **Schedule CU Changes** in the **Operation** column.
#. On the displayed page, click **Modify** in the **Operation** column. In the displayed dialog box, modify the task parameters as needed.

Deleting a Scheduled Task
-------------------------

If you do not need the task anymore, delete the task on the **Schedule CU Changes** page.

#. In the navigation pane of the DLI management console, choose **Resources** >\ **Queue Management**.
#. Locate the queue for which you want to schedule a periodic task for elastic scaling, and choose **More** > **Schedule CU Changes** in the **Operation** column.
#. On the displayed page, click **Delete** in the **Operation** column. In the displayed dialog box, click **OK**.
