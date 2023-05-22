:original_name: dli_01_0443.html

.. _dli_01_0443:

Modifying the CIDR Block
========================

If the CIDR block of the DLI queue conflicts with that of the user data source, you can change the CIDR block of the queue.

.. note::

   If the queue whose CIDR block is to be modified has jobs that are being submitted or running, or the queue has been bound to enhanced datasource connections, the CIDR block cannot be modified.

Procedure
---------

#. On the left of the DLI management console, click **Resources** >\ **Queue Management**.
#. Select the queue to be modified and click **Modify CIDR Block** in the **Operation** column.
#. Enter the required CIDR block and click **OK**.
