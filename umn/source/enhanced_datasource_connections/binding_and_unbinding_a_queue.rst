:original_name: dli_01_0514.html

.. _dli_01_0514:

Binding and Unbinding a Queue
=============================

Constraints
-----------

-  The CIDR block of the DLI queue that is bound with a datasource connection cannot overlap with that of the data source.
-  The default queue cannot be bound with a connection.

Binding a Queue
---------------

Before using an enhanced datasource connection, you must bind a queue and ensure that the VPC peering connection is in the **Active** state.

-  Method 1

   In the **Enhanced** tab, select a connection and click **Bind Queue** in the **Operation** column. In the dialog box that is displayed, select the queues to be bound and click **OK**.

-  Method 2

   Click the name of the selected connection. The **Details** page is displayed. Click **Create** in the upper left corner. In the dialog box that is displayed, select the queues to be bound and click **OK**.

Viewing Details about a Bound Queue
-----------------------------------

On the **Enhanced** tab page, select a connection and click the connection name to view the information about the bound queue.

.. table:: **Table 1** Parameters in the details list of datasource connection queues

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                          |
   +===================================+======================================================================================================================================================================================================================================================================================================================================================+
   | VPC Peering ID                    | ID of the VPC peering connection created in the cluster to which the queue belongs.                                                                                                                                                                                                                                                                  |
   |                                   |                                                                                                                                                                                                                                                                                                                                                      |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                      |
   |                                   |    A VPC peering connection is created for each queue bound to an enhanced datasource connection. The VPC peering connection is used for cross-VPC communication. Ensure that the security group used by the data source allows access from the DLI queue CIDR block, and do not delete the VPC peering connection during the datasource connection. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                              | Name of a bound queue.                                                                                                                                                                                                                                                                                                                               |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Connection Status                 | Datasource connection status. The following three statuses are available:                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                      |
   |                                   | -  Creating                                                                                                                                                                                                                                                                                                                                          |
   |                                   | -  Active                                                                                                                                                                                                                                                                                                                                            |
   |                                   | -  Failed                                                                                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                      |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                      |
   |                                   |    If the connection status is **Failed**, click on the left to view the detailed error information.                                                                                                                                                                                                                                                 |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Updated                           | Time when a connection is updated. The connections in the connection list can be displayed according to the update time in ascending or descending order.                                                                                                                                                                                            |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | **Unbind Queue**: This operation is used to unbind a datasource connection from a queue.                                                                                                                                                                                                                                                             |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Unbinding a Queue
-----------------

If you do not need to use an enhanced datasource connection, you can unbind the queue from it to release resources.

-  Method 1

   On the **Enhanced** tab page, select a connection and click **More** > **Unbind Queue** in the **Operation** column. In the dialog box that is displayed, select the queues to be unbound and click **OK**.

-  Method 2

   Click the name of the target connection. The **Details** page is displayed. Select the queue to be unbound and click **Unbind Queue** in the **Operation** column. In the displayed dialog box, confirm the queue to be unbound and click **Yes**.
