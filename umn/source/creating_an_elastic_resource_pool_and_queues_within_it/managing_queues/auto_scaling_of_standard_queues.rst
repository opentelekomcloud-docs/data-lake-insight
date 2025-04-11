:original_name: dli_01_0487.html

.. _dli_01_0487:

Auto Scaling of Standard Queues
===============================

Prerequisites
-------------

Newly created queues need to run jobs before they can be scaled in or out.

.. note::

   The operations described in this section only apply to standard queues.

Notes and Constraints
---------------------

-  Queues with 16 CUs do not support scale-out or scale-in.

-  Queues with 64 CUs do not support scale-in.

-  If **Status of queue xxx is assigning, which is not available** is displayed on the **Elastic Scaling** page, the queue can be scaled only after the queue resources are allocated.

-  If there are not enough physical resources, a queue may not be able to scale out to the desired target size.

-  The system does not guarantee that a queue will be scaled in to the desired target size. Typically, the system checks the resource usage before scaling in the queue to determine if there is enough space for scaling in. If the existing resources cannot be scaled in according to the minimum scaling step, the queue may not be scaled in successfully or only partially.

   The scaling step may vary depending on the resource specifications, usually 16 CUs, 32 CUs, 48 CUs, 64 CUs, etc.

   For example, if the queue size is 48 CUs and job execution uses 18 CUs, the remaining 30 CUs do not meet the requirement for scaling in by the minimum step of 32 CUs. If a scaling in task is executed, it will fail.

Scaling Out
-----------

If the current queue specifications do not meet service requirements, you can add the number of CUs to scale out the queue.

.. note::

   Scale-out is time-consuming. After you perform scale-out on the **Elastic Scaling** page of DLI, wait for about 10 minutes. The duration is related to the CU amount to add. After a period of time, refresh the **Queue Management** page and check whether values of **Specifications** and **Actual CUs** are the same to determine whether the scale-out is successful. Alternatively, on the **Job Management** page, check the status of the **SCALE_QUEUE** SQL job. If the job status is **Scaling**, the queue is being scaled out.

The procedure is as follows:

#. In the navigation pane on the left of the DLI management console, choose **Resources** > **Queue Management**.
#. Select the queue to be scaled out, click **More** in the **Operation** column, and select **Elastic Scaling**.
#. On the displayed page, select **Scale-out** for **Operation** and set the scale-out amount.
#. Click .

Scaling In
----------

If the current queue specifications are too much for your computing service, you can reduce the number of CUs to scale in the queue.

.. note::

   -  Scale-in is time-consuming. After you perform scale-in on the **Elastic Scaling** page of DLI, wait for about 10 minutes. The duration is related to the CU amount to reduce. After a period of time, refresh the **Queue Management** page and check whether values of **Specifications** and **Actual CUs** are the same to determine whether the scale-in is successful. Alternatively, on the **Job Management** page, check the status of the **SCALE_QUEUE** SQL job. If the job status is **Scaling**, the queue is being scaled in.
   -  By default, the minimum number of CUs is **16**. That is, when the queue specifications are **16 CUs**, you cannot scale in the queue.

The procedure is as follows:

#. In the navigation pane on the left of the DLI management console, choose **Resources** > **Queue Management**.
#. Select the queue to be scaled in, click **More** in the **Operation** column, and select **Elastic Scaling**.
#. On the displayed page, select **Scale-in** for **Operation** and set the scale-in amount.
#. Click .
