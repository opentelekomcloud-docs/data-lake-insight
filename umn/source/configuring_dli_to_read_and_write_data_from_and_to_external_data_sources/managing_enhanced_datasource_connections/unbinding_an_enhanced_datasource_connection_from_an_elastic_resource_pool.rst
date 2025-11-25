:original_name: dli_01_0555.html

.. _dli_01_0555:

Unbinding an Enhanced Datasource Connection from an Elastic Resource Pool
=========================================================================

Scenario
--------

Unbind an enhanced datasource connection from an elastic resource pool that does not need to access a data source through an enhanced datasoruce connection.

Notes and Constraints
---------------------

If the status of the VPC peering connection created for binding an enhanced datasource connection to an elastic resource pool is **Failed**, the elastic resource pool cannot be unbound.

Procedure
---------

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Datasource Connections**.
#. On the **Enhanced** tab page displayed, use either of the following methods to unbind an enhanced datasource connection from an elastic resource pool:

   -  Method 1:

      a. Locate your desired enhanced datasource connection, click **More** in the **Operation** column, and select **Unbind Resource Pool**.
      b. In the **Unbind Resource Pool** dialog box, select the resource pool to be unbound for **Resource Pool**.
      c. Click **OK**.

   -  Method 2:

      a. Click your desired enhanced datasource connection in the list.
      b. Locate your desired resource pool and click **Unbind Resource Pool** in the **Operation** column.
      c. Click **OK**.
