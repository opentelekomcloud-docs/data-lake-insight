:original_name: dli_01_0566.html

.. _dli_01_0566:

Allocating to an Enterprise Project
===================================

An enterprise project is a cloud resource management approach that allows organizations to plan resources based on their organizational structure. It enables unified management of resources distributed across different regions under specific enterprise projects. Additionally, user groups and users with varying permissions can be assigned to each enterprise project.

DLI allows you to select an enterprise project when creating an elastic resource pool. This section explains how to bind or modify the enterprise project for a DLI elastic resource pool.

.. note::

   Changing the enterprise project of an elastic resource pool will also update the enterprise project of its associated queue resources.

   Here, the elastic resource pool only supports adding queues from the same enterprise project.

Prerequisites
-------------

You have logged in to the Enterprise Project Management Service console and created an enterprise project.

Binding an Enterprise Project
-----------------------------

When creating an elastic resource pool, you can select a created enterprise project for **Enterprise Project**.

Alternatively, you can click **Create Enterprise Project** to go to the Enterprise Project Management Service console to create an enterprise project and check existing ones.

For details about how to create a queue, see :ref:`Creating an Elastic Resource Pool and Creating Queues Within It <dli_01_0505>`.

Modifying an Enterprise Project
-------------------------------

You can modify the enterprise project bound to a created cluster as needed.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. In the elastic resource pool list, locate the elastic resource pool for which you want to modify the enterprise project, click **More** in the **Operation** column, and select **Allocate to Enterprise Project**.

#. In the **Modify Enterprise Project** dialog box displayed, select an enterprise project.

   Alternatively, you can click **Create Enterprise Project** to go to the Enterprise Project Management Service console to create an enterprise project and check existing ones.

#. After the modification, click **OK** to save the enterprise project information of the elastic resource pool.

Related Operations
------------------

For details about how to modify the enterprise project of a queue, see :ref:`Allocating a Queue to an Enterprise Project <dli_01_0565>`.
