:original_name: dli_01_0006.html

.. _dli_01_0006:

Creating an Enhanced Datasource Connection
==========================================

Scenario
--------

Create an enhanced datasource connection for DLI to access, import, query, and analyze data of other data sources.

For example, to connect DLI to the MRS, RDS, CSS, Kafka, or GaussDB(DWS) data source, you need to enable the network between DLI and the VPC of the data source.

Create an enhanced datasource connection on the console.

Notes and Constraints
---------------------

.. table:: **Table 1** Notes and constraints on enhanced datasource connections

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                                                                                         |
   +===================================+=====================================================================================================================================================================================+
   | Use case                          | -  Datasource connections cannot be created for the **default** queue.                                                                                                              |
   |                                   | -  Flink jobs can directly access DIS, OBS, and SMN data sources without using datasource connections.                                                                              |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Permission                        | -  **VPC Administrator** permissions are required for enhanced connections to use VPCs, subnets, routes, VPC peering connections.                                                   |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Usage                             | -  If you use an enhanced datasource connection, the CIDR block of the elastic resource pool or queue cannot overlap with that of the data source.                                  |
   |                                   | -  Only queues bound with datasource connections can access datasource tables.                                                                                                      |
   |                                   | -  Datasource tables do not support the preview function.                                                                                                                           |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Connectivity check                | -  When checking the connectivity of datasource connections, the notes and constraints on IP addresses are:                                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The IP address must be valid, which consists of four decimal numbers separated by periods (.). The value ranges from 0 to 255.                                                |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  During the test, you can add a port after the IP address and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535.  |
   |                                   |                                                                                                                                                                                     |
   |                                   |       For example, **192.168.**\ *xx*\ **.**\ *xx* or **192.168.**\ *xx*\ **.**\ *xx*\ **:8181**.                                                                                   |
   |                                   |                                                                                                                                                                                     |
   |                                   | -  When checking the connectivity of datasource connections, the notes and constraints on domain names are:                                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The domain name can contain 1 to 255 characters. Only letters, numbers, underscores (_), and hyphens (-) are allowed.                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The top-level domain name must contain at least two letters, for example, **.com**, **.net**, and **.cn**.                                                                    |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  During the test, you can add a port after the domain name and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535. |
   |                                   |                                                                                                                                                                                     |
   |                                   |       Example: **example.com:8080**                                                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Process
-------


.. figure:: /_static/images/en-us_image_0000001620789569.png
   :alt: **Figure 1** Enhanced datasource connection creation flowchart

   **Figure 1** Enhanced datasource connection creation flowchart

Prerequisites
-------------

-  An elastic resource pool or queue has been created.
-  You have obtained the VPC, subnet, private IP address, port, and security group information of the external data source.
-  The security group of the external data source has allowed access from the CIDR block of the elastic resource pool or queue.

Procedure
---------

#. **Create an Enhanced Datasource Connection**

   a. Log in to the DLI management console.

   b. In the navigation pane on the left, choose **Datasource Connections**.

   c. On the displayed **Enhanced** tab, click **Create**.

      Set parameters based on :ref:`Table 2 <dli_01_0006__table9495521165111>`.

      .. _dli_01_0006__table9495521165111:

      .. table:: **Table 2** Parameters

         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                                         |
         +===================================+=====================================================================================================================================================================================================================================================================================================================+
         | Connection Name                   | Name of the created datasource connection.                                                                                                                                                                                                                                                                          |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | -  Only letters, numbers, and underscores (_) are allowed. The parameter must be specified.                                                                                                                                                                                                                         |
         |                                   | -  A maximum of 64 characters are allowed.                                                                                                                                                                                                                                                                          |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Resource Pool                     | It binds an elastic resource pool or queue that uses a datasource connection. This parameter is optional.                                                                                                                                                                                                           |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | In regions where this function is available, an elastic resource pool with the same name is created by default for the queue created in "Creating a Queue."                                                                                                                                                         |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    Before using an enhanced datasource connection, you must bind a queue and ensure that the VPC peering connection is in the **Active** state.                                                                                                                                                                     |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | VPC                               | VPC used by the data source.                                                                                                                                                                                                                                                                                        |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Subnet                            | Subnet used by the data source.                                                                                                                                                                                                                                                                                     |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Host Information                  | In this text field, you can configure the mapping between host IP addresses and domain names so that jobs can only use the configured domain names to access corresponding hosts. This parameter is optional.                                                                                                       |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | For example, when accessing the HBase cluster of MRS, you need to configure the host name (domain name) and IP address of the ZooKeeper instance. Enter one record in each line in the format of *IP address* *Host name*\ **/**\ *Domain name*.                                                                    |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | Example:                                                                                                                                                                                                                                                                                                            |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | 192.168.0.22 node-masterxxx1.com                                                                                                                                                                                                                                                                                    |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | 192.168.0.23 node-masterxxx2.com                                                                                                                                                                                                                                                                                    |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | For details about how to obtain host information, see :ref:`How Do I Obtain MRS Host Information? <dli_01_0013__section3607172865810>`.                                                                                                                                                                             |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Tags                              | Tags used to identify cloud resources. A tag includes the tag key and tag value. If you want to use the same tag to identify multiple cloud resources, that is, to select the same tag from the drop-down list box for all services, you are advised to create predefined tags on the Tag Management Service (TMS). |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    -  A maximum of 20 tags can be added.                                                                                                                                                                                                                                                                            |
         |                                   |    -  Only one tag value can be added to a tag key.                                                                                                                                                                                                                                                                 |
         |                                   |    -  The key name in each resource must be unique.                                                                                                                                                                                                                                                                 |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | -  Tag key: Enter a tag key name in the text box.                                                                                                                                                                                                                                                                   |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |       A tag key can contain a maximum of 128 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                                  |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | -  Tag value: Enter a tag value in the text box.                                                                                                                                                                                                                                                                    |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |       A tag value can contain a maximum of 255 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed.                                                                                                                                                                          |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   d. Click **OK**.

      After the creation is complete, the enhanced datasource connection is in the **Active** state, indicating that the connection is successfully created.

#. **Security Group Where the Data Source Belongs Allows Access from the CIDR Block of the Elastic Resource Pool**

   a. On the DLI management console, obtain the network segment of the elastic resource pool or queue.

      Choose **Resources** > **Queue Management** from the left navigation pane. On the page displayed, locate the queue on which jobs are running, and click the button next to the queue name to obtain the CIDR block of the queue.

   b. Log in to the VPC console and find the VPC the data source belongs to.

   c. On the network console, choose **Virtual Private Cloud** > **Network Interfaces**. On the **Network Interfaces** tab page displayed, search for the security group name, click **More** in the **Operation** column, and select **Change Security Group**.

   d. In the navigation pane on the left, choose **Access Control** > **Security Groups**.

   e. Click the name of the security group to which the external data source belongs.

   f. Click the **Inbound Rules** tab and add a rule to allow access from the CIDR block of the queue.

      Configure the inbound rule parameters according to :ref:`Table 3 <dli_01_0006__table4276105765618>`.

      .. _dli_01_0006__table4276105765618:

      .. table:: **Table 3** Inbound rule parameters

         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
         | Parameter             | Description                                                                                                                                                                 | Example Value                                                                            |
         +=======================+=============================================================================================================================================================================+==========================================================================================+
         | Priority              | Priority of a security group rule.                                                                                                                                          | 1                                                                                        |
         |                       |                                                                                                                                                                             |                                                                                          |
         |                       | The priority value ranges from 1 to 100. The default value is **1**, indicating the highest priority. A smaller value indicates a higher priority of a security group rule. |                                                                                          |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
         | Action                | Action of the security group rule.                                                                                                                                          | Allow                                                                                    |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
         | Protocol & Port       | -  Network protocol. The value can be **All**, **TCP**, **UDP**, **ICMP**, or **GRE**.                                                                                      | In this example, select **TCP**. Leave the port blank or set it to the data source port. |
         |                       | -  Port: Port or port range over which the traffic can reach your instance. The port ranges from 1 to 65535.                                                                |                                                                                          |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
         | Type                  | Type of IP addresses.                                                                                                                                                       | IPv4                                                                                     |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
         | Source                | Allows access from IP addresses or instances in another security group.                                                                                                     | In this example, enter the obtained queue CIDR block.                                    |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
         | Description           | Supplementary information about the security group rule. This parameter is optional.                                                                                        | \_                                                                                       |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+

#. **Test the Connectivity Between the DLI Queue and the Data Source**

   a. Obtain the private IP address and port number of the data source.

      Take the RDS data source as an example. On the **Instances** page, click the target DB instance. On the page displayed, locate the **Connection Information** pane and view the private IP address. In the **Connection Information** pane, locate the **Database Port** to view the port number of the RDS DB instance.

   b. In the navigation pane of the DLI management console, choose **Resources** > **Queue Management**.

   c. Locate the queue bound with the enhanced datasource connection, click **More** in the **Operation** column, and select **Test Address Connectivity**.

   d. Enter the data source connection address and port number to test the network connectivity.

      Format: *IP address*\ **:**\ *Port number*

      .. caution::

         Before testing the connection, ensure that the security group of the external data source has allowed access from the CIDR block of the queue.
