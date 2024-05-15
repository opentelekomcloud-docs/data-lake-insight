:original_name: dli_01_0507.html

.. _dli_01_0507:

Setting CUs
===========

CU settings are used to control the maximum and minimum CU ranges for elastic resource pools to avoid unlimited resource scaling.

For example, an elastic resource pool has a maximum of 256 and two queues, and each queue must have at least 64 CUs. If you want to add another queue that needs at lest 256 CUs to the elastic resource pool, the operation is not allowed due to the maximum CUs of the elastic resource pool.

Precautions
-----------

-  In any time segment of a day, the total minimum CUs of all queues in an elastic resource pool cannot be more than the minimum CUs of the pool.
-  In any time segment of a day, the maximum CUs of any queue in an elastic resource pool cannot be more than the maximum CUs of the pool.
-  When you change the minimum CUs of a created elastic resource pool, ensure that the value is no more than the current CU value. Otherwise, the modification fails.


Setting CUs
-----------

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.
#. Locate the row that contains the desired elastic resource pool, click **More** in the **Operation** column, and select **Set CUs**.
#. In the **Set CUs** dialog box, set the minimum CUs on the left and the maximum CUs on the right. Click **OK**.

FAQ
---

-  **How Do I Change the Minimum CUs of the Existing Queues in an Elastic Resource Pool If the Total CUs of the Queues Equal the Minimum CUs of the Elastic Resource Pool?**

   Answer:

   -  Step 1: Increase the maximum number of CUs of the existing queues so that the current number of CUs of the elastic resource pool is no less than the target minimum number of CUs (the sum of the minimum number of CUs of the queues you want to change to).

      .. note::

         If the maximum number of CUs of the elastic resource pool is equal to its minimum number of CUs, increase the maximum number of CUs.

   -  Step 2: Set the minimum CUs of the elastic resource pool.
   -  Step 3: Change the minimum CUs of the existing queues in the elastic resource pool.

-  **How Do I Add Queues to an Elastic Resource Pool If the Total CUs of the Queues Equal the Minimum CUs of the Elastic Resource Pool?**

   Answer:

   -  .. _dli_01_0507__li9752335133715:

      Step 1: Increase the maximum number of CUs of the existing queues so that the current number of CUs of the elastic resource pool is no less than the target minimum number of CUs (the sum of the minimum number of CUs of the queues you want to change to).

      .. note::

         If the maximum number of CUs of the elastic resource pool is equal to its minimum number of CUs, increase the maximum number of CUs.

   -  Step 2: Set the minimum CUs of the elastic resource pool.

   -  Step 3: Add queues to the elastic resource pool.

   -  Step 4: Restore the maximum CUs of the queues you have increased in :ref:`Step 1 <dli_01_0507__li9752335133715>`.
