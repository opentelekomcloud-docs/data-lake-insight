:original_name: dli_01_0009.html

.. _dli_01_0009:

Binding an Enhanced Datasource Connection to an Elastic Resource Pool
=====================================================================

Scenario
--------

To connect other resource pools to data sources through enhanced datasource connections, bind enhanced datasource connections to resource pools on the **Enhanced** tab page.

Notes and Constraints
---------------------

-  The CIDR block of the DLI queue to be bound with a datasource connection cannot overlap with that of the data source.
-  The **default** queue preset in the system cannot be bound with a datasource connection.

Procedure
---------

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Datasource Connections**.
#. On the **Enhanced** tab page displayed, bind an enhanced datasource connection to an elastic resource pool:

   a. Locate your desired enhanced datasource connection, click **More** in the **Operation** column, and select **Bind Resource Pool**.
   b. In the **Bind Resource Pool** dialog box, select the resource pool to be bound for **Resource Pool**.
   c. Click **OK**.

#. View the connection status on the **Enhanced** tab page.

   -  After an enhanced datasource connection is created, the status is **Active**, but it does not indicate that the queue is connected to the data source. Go to the queue management page to check whether the data source is connected. The procedure is as follows:

      a. In the navigation pane on the left, choose **Resources** > **Queue Management**. On the page displayed, locate a desired queue.
      b. Click **More** in the **Operation** column and select **Test Address Connectivity**.
      c. Enter the IP address and port number of the data source.

   -  On the details page of an enhanced datasource connection, you can view information about the VPC peering connection.

      -  VPC peering ID: ID of the VPC peering connection created in the cluster to which the queue belongs.

         A VPC peering connection is created for each queue bound to an enhanced datasource connection. The VPC peering connection is used for cross-VPC communication. Ensure that the security group used by the data source allows access from the CIDR block of the DLI queue, and do not delete the VPC peering connection during the datasource connection.

      -  Status of the VPC peering connection:

         The status of a datasource connection can be **Creating**, **Active**, or **Failed**.

         If the connection status is **Failed**, click |image1| on the left to view the detailed error information.

.. |image1| image:: /_static/images/en-us_image_0000001620741493.png
