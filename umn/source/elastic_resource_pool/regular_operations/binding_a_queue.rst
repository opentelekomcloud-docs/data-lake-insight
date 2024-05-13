:original_name: dli_01_0530.html

.. _dli_01_0530:

Binding a Queue
===============

Scenario
--------

If you want a queue to use resources in an elastic resource pool, bind the queue to the pool.

You can click **Associate Queue** on the **Resource Pool** page to bind a queue to an elastic resource pool, or bind a queue on the **Queue Management** page.

.. note::

   Elastic resource pools support only Flink 1.10 or later. If jobs using Flink 1.7 run on a queue that is bound to an elastic resource pool, errors may occur due to incompatibility.

Prerequisites
-------------

-  Both the elastic resource pool and queue are available.
-  The queue you want to bind must be a dedicated queue in pay-per-use billing mode.
-  No resources are frozen.
-  Only queues under the same enterprise project can be bound to an elastic resource pool.

Associating a Queue
-------------------

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.
#. Locate the row that contains the desired elastic resource pool, click **More** in the **Operation** column, and select **Associate Queue**.
#. In the displayed dialog box, select the desired queue and click **OK**.

Allocating a Queue to an Elastic Resource Pool
----------------------------------------------

#. In the navigation pane on the left, choose **Resources** > **Queue Management**.
#. Locate the target queue and choose **More** > **Bind Resource Pool** in the **Operation** column.
#. Select the desired elastic resource pool and click **OK**.
