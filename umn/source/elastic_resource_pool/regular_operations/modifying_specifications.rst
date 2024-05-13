:original_name: dli_01_0524.html

.. _dli_01_0524:

Modifying Specifications
========================

Scenario
--------

If CUs of a yearly/monthly elastic resource pool cannot meet your service requirements, you can modify the CUs. In this case, you will be charged based on the number of CUs exceeding that of the yearly/monthly elastic resource pool.

For example, you have purchased an elastic resource pool with 64 CUs, and you find that most time data processing needs 128 CUs. You can add 64 CUs to the elastic resource pool and be billed based on a CU/hour basis. To save more, you can scale up your elastic resource pool to 128 CUs and be billed on a yearly/monthly basis for the 128-CU package.

Precautions
-----------

Currently, only yearly/monthly elastic resource pools can be scaled.

Scaling Up
----------

#. In the navigation pane on the left of the console, choose **Resources** > **Resource Pool**.

#. Select the elastic resource pool you want and choose **More** > **Modify Specifications** in the **Operation** column.

#. In the **Modify Specifications** dialog page, set **Operation** to **Scale-out** and specify the number of CUs you want to add.

#. Confirm the changes and click **OK**.

#. Choose **Job Management** > **SQL Jobs** to view the status of the SCALE_POOL SQL job.

   If the job status is **Scaling**, the elastic resource pool is scaling up. Wait until the job status changes to **Finished**.

Scaling Down
------------

.. note::

   By default, the minimum number of CUs is **16**. That is, when the specifications of an elastic resource pool are **16 CUs**, you cannot scale the pool down.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. Select the elastic resource pool you want and choose **More** > **Modify Specifications** in the **Operation** column.

#. In the **Modify Specifications** dialog page, set **Operation** to **Scale-in** and specify the number of CUs you want to add.

#. Confirm the changes and click **OK**.

#. Choose **Job Management** > **SQL Jobs** to view the status of the SCALE_POOL SQL job.

   If the job status is **Scaling**, the elastic resource pool is scaling down. Wait until the job status changes to **Finished**.
