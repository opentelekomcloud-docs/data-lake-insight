:original_name: dli_01_0565.html

.. _dli_01_0565:

Allocating a Queue to an Enterprise Project
===========================================

You can create enterprise projects matching the organizational structure of your enterprises to centrally manage cloud resources across regions by project. Then you can create user groups and users with different permissions and add them to enterprise projects.

DLI allows you to select an enterprise project when creating a queue. This section describes how to bind a DLI queue to and modify an enterprise project.

.. note::

   Currently, enterprise projects can be modified only for queues that have not been added to elastic resource pools.

Prerequisites
-------------

You have logged in to the Enterprise Project Management Service console and created an enterprise project.

Modifying an Enterprise Project
-------------------------------

You can change the enterprise project associated with a queue that has been created.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Queue Management**.

#. In the queue list, locate the queue for which you want to modify the enterprise project, click **More** in the **Operation** column, and select **Modify Enterprise Project**.

#. In the **Modify Enterprise Project** dialog box displayed, select an enterprise project.

   Alternatively, you can click **Create Enterprise Project** to go to the Enterprise Project Management Service console to create an enterprise project and check existing ones.

#. After the modification, click **OK** to save the enterprise project information of the queue.

Related Operations
------------------

For details about how to modify the enterprise project of an elastic resource pool, see :ref:`Allocating to an Enterprise Project <dli_01_0566>`.
