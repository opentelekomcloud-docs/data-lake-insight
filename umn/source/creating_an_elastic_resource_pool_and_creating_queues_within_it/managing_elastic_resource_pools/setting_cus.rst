:original_name: dli_01_0507.html

.. _dli_01_0507:

Setting CUs
===========

CU settings are used to control the maximum and minimum CU ranges for elastic resource pool scaling to prevent unlimited resource expansion risks.

For example, if the current maximum CUs of an elastic resource pool is 256 CUs, and two queues have been added with their scaling policies set to a minimum of 64 CUs each, attempting to add another queue requiring a minimum of 256 CUs would fail due to the constraints imposed by the maximum CU setting.

Notes and Constraints
---------------------

The minimum value of the CU range of the elastic resource pool cannot exceed the current actual CUs.

For example, if you want to raise the minimum value of the CU range (for example, from 64 CUs to 80 CUs), you must first ensure that the actual CUs are at least 80.

For methods on adjusting the actual CUs, refer to :ref:`Scaling Out or In an Elastic Resource Pool <dli_01_0686>`.

Precautions
-----------

-  In any time segment of a day, the total minimum CUs of all queues in an elastic resource pool cannot be more than the minimum CUs of the pool.
-  In any time segment of a day, the maximum CUs of any queue in an elastic resource pool cannot be more than the maximum CUs of the pool.
-  When adjusting the minimum CUs of a created elastic resource pool, the minimum CUs must be less than or equal to the actual CUs of the pool. Otherwise, the modification will fail.
-  The adjustment of the CU range of a queue, the change of the elastic resource pool specifications, and the CU setting of an elastic resource pool take effect on the next hour.
-  You can add queues to adjust the actual CUs of an elastic resource pool. The adjustment takes effect immediately.

Procedure
---------

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
