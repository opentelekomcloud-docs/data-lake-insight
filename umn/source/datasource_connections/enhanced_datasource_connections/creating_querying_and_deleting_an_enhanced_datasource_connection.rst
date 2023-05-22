:original_name: dli_01_0006.html

.. _dli_01_0006:

Creating, Querying, and Deleting an Enhanced Datasource Connection
==================================================================

Creating an Enhanced Datasource Connection
------------------------------------------

The following describes how to create a datasource HBase connection for MRS.

.. note::

   Only enhanced datasource connection to MRS HBase is supported.

#. Apply for a cluster in MRS.

   If a cluster is available, you do not need to apply for one.

#. In the navigation pane of the DLI management console, choose **Datasource Connections**.

#. Click the **Enhanced** tab and click **Create** in the upper left corner.

   Enter the **Connection Name**, select the **Bind Queue** (optional), **VPC**, and **Subnet**, and enter the **Host Information** (optional). For details about the parameters, see :ref:`Table 1 <dli_01_0006__table24931148155220>`.

   .. _dli_01_0006__table24931148155220:

   .. table:: **Table 1** Parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                          |
      +===================================+======================================================================================================================================================================================================================+
      | Connection Name                   | Name of the created datasource connection.                                                                                                                                                                           |
      |                                   |                                                                                                                                                                                                                      |
      |                                   | -  The name can contain only letters, digits, and underscores (_). The parameter must be specified.                                                                                                                  |
      |                                   | -  A maximum of 64 characters are allowed.                                                                                                                                                                           |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Resource Pool                     | It binds an elastic resource pool or queue that uses a datasource connection. This parameter is optional.                                                                                                            |
      |                                   |                                                                                                                                                                                                                      |
      |                                   | In regions where this function is available, a resource pool with the same name is created by default for the queue created in "Creating a Queue."                                                                   |
      |                                   |                                                                                                                                                                                                                      |
      |                                   | .. note::                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                      |
      |                                   |    Before using an enhanced datasource connection, you must bind a queue and ensure that the VPC peering connection is in the **Active** state.                                                                      |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | VPC                               | VPC used by the destination data source.                                                                                                                                                                             |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Subnet                            | Subnet used by the destination data source.                                                                                                                                                                          |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Host Information                  | (Optional) When connecting to the HBase cluster of MRS, enter the host name and IP address of the ZooKeeper instance. Enter one record in each line. The format is as follows: **IP address Host name/Domain name**. |
      |                                   |                                                                                                                                                                                                                      |
      |                                   | To obtain the host name and IP address of the MRS cluster, perform the following steps (with MRS3.x as an example):                                                                                                  |
      |                                   |                                                                                                                                                                                                                      |
      |                                   | a. Log in to the MRS management console.                                                                                                                                                                             |
      |                                   | b. In the navigation pane, choose **Clusters** > **Active Clusters**. Click the target cluster name to access the cluster details page.                                                                              |
      |                                   | c. Click **Component Management**.                                                                                                                                                                                   |
      |                                   | d. Click **Zookeeper**.                                                                                                                                                                                              |
      |                                   | e. Click the **Instance** tab to view the corresponding service IP address. You can select any service IP address.                                                                                                   |
      |                                   |                                                                                                                                                                                                                      |
      |                                   | .. note::                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                      |
      |                                   |    If the MRS cluster has multiple IP addresses, enter any service IP address when creating a datasource connection.                                                                                                 |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

#. To connect to Kafka, DWS, and RDS instances, add security group rules for the DLI network segment to the security group where the instances belong. The following uses RDS as an example:

   a. Choose **Resources** > **Queue Management** in the navigation pane. Select the target queue, and click |image1| to expand the row containing the target queue to view its CIDR block.
   b. On the **Instance Management** page of the RDS console, click the instance name. In the **Connection Information** area, view the port number of the RDS database instance.
   c. In the **Connection Information** area locate the **Security Group** and click the group name to switch to the security group management page. Select the **Inbound Rules** tab and click **Add Rule**. Set the priority to 1, protocol to TCP, port to the database port number, and source to the CIDR block of the DLI queue. Click **OK**.

#. Test the connectivity between the DLI queue and the connection instance. The following example describes how to test the connectivity between DLI and an RDS DB instance.

   a. On the **Instance Management** page, click the target DB instance. On the displayed page, locate the **Connection Information** pane and view the floating IP address. In the **Connection Information** pane, locate the **Database Port** to view the port number of the RDS DB instance.
   b. Go back to the DLI console. On the **Resources** > **Queue Management** page, locate the target queue. In the **Operation** column, click **More** and select **Test Address Connectivity**.
   c. Enter the connection address of the RDS DB instance and port number in the format of **IP address:port** to test the network connectivity.

Querying an Enhanced Datasource Connection
------------------------------------------

On the **Enhanced** tab page, you can enter the keyword of a connection name in the search box to search for the matching connection.

Viewing Details
---------------

On the **Enhanced** tab page, select a connection and click |image2| to view its details. The connection ID and host information are displayed.

Deleting an Enhanced Datasource Connection
------------------------------------------

On the **Enhanced** tab page, click **Delete Connection** in the **Operation** column to delete an unnecessary connection.

.. note::

   A connection with **Connection Status** of **Creating** cannot be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001259009999.png
.. |image2| image:: /_static/images/en-us_image_0000001105517798.png
