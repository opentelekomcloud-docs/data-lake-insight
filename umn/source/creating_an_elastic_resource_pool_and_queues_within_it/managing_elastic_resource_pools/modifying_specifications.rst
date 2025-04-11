:original_name: dli_01_0524.html

.. _dli_01_0524:

Modifying Specifications
========================

Scenario
--------

If the current specifications of your elastic resource pool do not meet your service needs, you can modify them using the change yearly/monthly CUs function.

Scaling Out
-----------

#. In the navigation pane on the left of the console, choose **Resources** > **Resource Pool**.

#. Locate the elastic resource pool you want to scale out, click **More** in the **Operation** column, and select **Modify Yearly/Monthly CU**.

#. On the **Modify Yearly/Monthly CU** page, set **Operation** to **Scale-out** and specify the number of CUs you want to add.

#. Confirm the changes and click **OK**.

#. Choose **Job Management** > **SQL Jobs** to view the status of the SCALE_POOL SQL job.

   If the job status is **Scaling**, the elastic resource pool is scaling out. Wait until the job status changes to **Finished**.

Scaling In
----------

.. note::

   By default, the minimum number of CUs is **16**. That is, when the specifications of an elastic resource pool are **16 CUs**, you cannot scale the pool down.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. Locate the elastic resource pool you want to scale in, click **More** in the **Operation** column, and select **Modify Yearly/Monthly CU**.

#. On the **Modify Yearly/Monthly CU** page, set **Operation** to **Scale-in** and specify the number of CUs you want to decrease.

#. Confirm the changes and click **OK**.

#. Choose **Job Management** > **SQL Jobs** to view the status of the SCALE_POOL SQL job.

   If the job status is **Scaling**, the elastic resource pool is scaling in. Wait until the job status changes to **Finished**.
