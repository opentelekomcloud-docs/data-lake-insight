:original_name: dli_01_0443.html

.. _dli_01_0443:

Changing the CIDR Block for a Standard Queue
============================================

If the CIDR block of the DLI queue conflicts with that of the user data source, you can change the CIDR block of the queue.

If the queue whose CIDR block is to be modified has jobs that are being submitted or running, or the queue has been bound to enhanced datasource connections, the CIDR block cannot be modified.

.. note::

   The operations described in this section only apply to standard queues.

Procedure
---------

#. In the navigation pane on the left of the DLI management console, choose **Resources** > **Queue Management**.
#. Select the queue to be modified and click **Modify CIDR Block** in the **Operation** column.
#. Enter the required CIDR block and click **OK**. After the CIDR block of the queue is successfully changed, wait for 5 to 10 minutes until the cluster to which the queue belongs is restarted and then run jobs on the queue.
