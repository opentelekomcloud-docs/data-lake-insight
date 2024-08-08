:original_name: dli_01_0563.html

.. _dli_01_0563:

Setting Queue Properties
========================

Scenario
--------

DLI allows you to set properties for queues.

You can set Spark driver parameters to improve the scheduling efficiency of queues.

This section describes how to set queue properties on the management console.

Notes and Constraints
---------------------

-  Only SQL queues of the Spark engine support configuring queue properties.
-  Setting queue properties is only supported after the queue has been created.
-  Currently, only queue properties related to the Spark driver can be set.
-  Queue properties cannot be set in batches.
-  For a queue in an elastic resource pool, if the minimum CUs of the queue is less than 16 CUs, both **Max. Spark Driver Instances** and **Max. Prestart Spark Driver Instances** set in the queue properties do not apply.

Procedure
---------

#. In the navigation pane of the DLI management console, choose **Resources** > **Queue Management**.

#. In the **Operation** column of the queue, choose **More** > **Set Property**.

#. Go to the queue property setting page and set property parameters. For details about the property parameters, see :ref:`Table 1 <dli_01_0563__table206971632142710>`.

   .. _dli_01_0563__table206971632142710:

   .. table:: **Table 1** Queue properties

      +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
      | Property                             | Description                                                                                                                                                                                                                              | Value Range                                                                                     |
      +======================================+==========================================================================================================================================================================================================================================+=================================================================================================+
      | Max. Spark Driver Instances          | Maximum number of Spark drivers can be started on this queue, including the Spark driver that is prestarted and the Spark driver that runs jobs.                                                                                         | -  For a 16-CU queue, the value is **2**.                                                       |
      |                                      |                                                                                                                                                                                                                                          | -  For a queue that has more than 16 CUs, the value range is [2, queue CUs/16].                 |
      |                                      |                                                                                                                                                                                                                                          | -  If the minimum CUs of the queue is less than 16 CUs, this configuration item does not apply. |
      +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
      | Max. Prestart Spark Driver Instances | Maximum number of Spark drivers can be prestarted on this queue. When the number of Spark drivers that run jobs exceeds the value of **Max. Concurrency per Instance**, the jobs are allocated to the Spark drivers that are prestarted. | -  For a 16-CU queue, the value range is 0 to 1.                                                |
      |                                      |                                                                                                                                                                                                                                          | -  For a queue that has more than 16 CUs, the value range is [2, queue CUs/16].                 |
      |                                      |                                                                                                                                                                                                                                          | -  If the minimum CUs of the queue is less than 16 CUs, this configuration item does not apply. |
      +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
      | Max. Concurrency per Instance        | Maximum number of jobs can be concurrently executed by a Spark driver. When the number of jobs exceeds the value of this parameter, the jobs are allocated to other Spark drivers.                                                       | 1-32                                                                                            |
      +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+

#. Click **OK**.
