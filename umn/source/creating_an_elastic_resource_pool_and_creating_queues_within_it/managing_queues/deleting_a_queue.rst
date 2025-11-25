:original_name: dli_01_0016.html

.. _dli_01_0016:

Deleting a Queue
================

You can delete a queue based on actual conditions.

.. note::

   -  This operation will fail if there are jobs in the **Submitting** or **Running** state on this queue.
   -  Deleting a queue does not cause table data loss in your database.

Procedure
---------

#. In the navigation pane on the left of the DLI management console, choose **Resources** > **Queue Management**.
#. Locate the row where the target queue locates and click **Delete** in the **Operation** column.

   .. note::

      If **Delete** in the **Operation** column is gray, the current user does not have the permission of deleting the queue. You can apply to the administrator for the permission.

#. In the displayed dialog box, click **OK**.
