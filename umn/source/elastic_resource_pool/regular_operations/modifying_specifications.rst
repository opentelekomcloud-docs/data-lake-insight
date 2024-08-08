:original_name: dli_01_0524.html

.. _dli_01_0524:

Modifying Specifications
========================

Scenario
--------

If the current specifications of your elastic resource pool do not meet your service needs, you can modify them using the change specifications function.

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
