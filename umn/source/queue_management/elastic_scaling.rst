:original_name: dli_01_0487.html

.. _dli_01_0487:

Elastic Scaling
===============

Prerequisites
-------------

Elastic scaling can be performed for a newly created queue only when there were jobs running in this queue.

Precautions
-----------

-  If **Status of queue xxx is assigning, which is not available** is displayed on the **Elastic Scaling** page, the queue can be scaled only after the queue resources are allocated.

Scaling Out
-----------

If the current queue specifications do not meet service requirements, you can add the number of CUs to scale out the queue.

.. note::

   Scale-out is time-consuming. After you perform scale-out on the **Elastic Scaling** page of DLI, wait for about 10 minutes. The duration is related to the CU amount to add. After a period of time, refresh the **Queue Management** page and check whether values of **Specifications** and **Actual CUs** are the same to determine whether the scale-out is successful. Alternatively, on the **Job Management** page, check the status of the **SCALE_QUEUE** SQL job. If the job status is **Scaling**, the queue is being scaled out.

The procedure is as follows:

#. On the left of the DLI management console, click **Resources** > **Queue Management**.
#. Select the queue to be scaled out and choose **More > Elastic Scaling** in the **Operation** column.
#. On the displayed page, select **Scale-out** for **Operation** and set the scale-out amount.
#. Click .

Scaling In
----------

If the current queue specifications are too much for your computing service, you can reduce the number of CUs to scale in the queue.

.. note::

   -  Scale-in is time-consuming. After you perform scale-in on the **Elastic Scaling** page of DLI, wait for about 10 minutes. The duration is related to the CU amount to reduce. After a period of time, refresh the **Queue Management** page and check whether values of **Specifications** and **Actual CUs** are the same to determine whether the scale-in is successful. Alternatively, on the **Job Management** page, check the status of the **SCALE_QUEUE** SQL job. If the job status is **Scaling**, the queue is being scaled in.
   -  The system may not fully scale in the queue to the target size. If the current queue is in use or the service volume of the queue is large, the scale-in may fail or only partial specifications may be reduced.
   -  By default, the minimum number of CUs is **16**. That is, when the queue specifications are **16 CUs**, you cannot scale in the queue.

The procedure is as follows:

#. On the left of the DLI management console, click **Resources** > **Queue Management**.
#. Select the queue to be scaled out and choose **More > Elastic Scaling** in the **Operation** column.
#. On the displayed page, select **Scale-in** for **Operation** and set the scale-in amount.
#. Click .
