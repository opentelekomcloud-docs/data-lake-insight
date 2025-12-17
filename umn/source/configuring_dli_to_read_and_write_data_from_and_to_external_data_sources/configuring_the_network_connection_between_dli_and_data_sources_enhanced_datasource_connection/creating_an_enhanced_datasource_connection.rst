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
   |                                   |    -  The IP address must be valid, which consists of four decimal digits separated by periods (.). The value ranges from 0 to 255.                                                 |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  During the test, you can add a port after the IP address and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535.  |
   |                                   |                                                                                                                                                                                     |
   |                                   |       For example, **192.168.**\ *xx*\ **.**\ *xx* or **192.168.**\ *xx*\ **.**\ *xx*\ **:8181**.                                                                                   |
   |                                   |                                                                                                                                                                                     |
   |                                   | -  When checking the connectivity of datasource connections, the notes and constraints on domain names are:                                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The domain name can contain 1 to 255 characters. Only letters, digits, underscores (_), and hyphens (-) are allowed.                                                          |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The top-level domain name must contain at least two letters, for example, **.com**, **.net**, and **.cn**.                                                                    |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  During the test, you can add a port after the domain name and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535. |
   |                                   |                                                                                                                                                                                     |
   |                                   |       Example: **example.com:8080**                                                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Procedure
---------

This part introduces the network connectivity solution for connecting a DLI elastic resource pool to a data source of a VPC using an enhanced datasource connection.

#. Creating an enhanced datasource connection: Establish a VPC peering connection to connect DLI and the data source's VPC network. For details, see :ref:`Creating an Enhanced Datasource Connection <dli_01_0006>`.

#. Configure routing rules: After an enhanced datasource connection is created, the CIDR block is automatically associated with the system default route without additional operations.

   Besides the system default route, you can add custom routing rules as needed to forward traffic destined for the target address to the specified next-hop address. For details, see :ref:`Adding a Route for an Enhanced Datasource Connection <dli_01_0014>`.

#. Testing network connectivity: Verify the connectivity between the queue and the data source's network. See :ref:`Testing the Network Connectivity Between a Queue and a Data Source <dli_01_0489>`.

For details about the data sources that support datasource connections, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.


.. figure:: /_static/images/en-us_image_0000002363115912.png
   :alt: **Figure 1** Using an enhanced datasource connection to establish network communication between a DLI elastic resource pool and a data source

   **Figure 1** Using an enhanced datasource connection to establish network communication between a DLI elastic resource pool and a data source

Prerequisites
-------------

-  An elastic resource pool or queue has been created. For details, see :ref:`Creating an Elastic Resource Pool and Creating Queues Within It <dli_01_0505>`.
-  You have obtained the VPC, subnet, private IP address, port, and security group information of the external data source.

Preparation: Allow the CIDR Block of the Elastic Resource Pool to Pass Through the Security Group Where the Data Source Is
--------------------------------------------------------------------------------------------------------------------------

#. On the DLI management console, obtain the CIDR block of the elastic resource pool.

   In the navigation pane on the left, choose **Resources** > **Resource Pool**. On the displayed page, locate your desired elastic resource pool, click |image1| to expand its details, and find its CIDR block.

#. Log in to the VPC console and find the VPC the data source belongs to.

#. On the network console, choose **Virtual Private Cloud** > **Network Interfaces**. On the **Network Interfaces** tab page displayed, search for the security group name, click **More** in the **Operation** column, and select **Change Security Group**.

#. In the navigation pane on the left, choose **Access Control** > **Security Groups**.

#. Click the name of the security group to which the external data source belongs.

#. On the **Inbound Rules** tab, add a rule to allow access from the queue network segment.

   Configure the inbound rule parameters according to :ref:`Table 2 <dli_01_0006__table4276105765618>`.

   .. _dli_01_0006__table4276105765618:

   .. table:: **Table 2** Inbound rule parameters

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

Step 1: Create an Enhanced Datasource Connection
------------------------------------------------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Datasource Connections**.

#. On the displayed **Enhanced** tab, click **Create**.

   Set parameters according to :ref:`Table 3 <dli_01_0006__table9495521165111>`.

   .. _dli_01_0006__table9495521165111:

   .. table:: **Table 3** Parameter descriptions

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                         |
      +===================================+=====================================================================================================================================================================================================================================================================================================================+
      | Connection Name                   | Name of the created datasource connection.                                                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  Only letters, digits, and underscores (_) are allowed. The parameter must be specified.                                                                                                                                                                                                                          |
      |                                   | -  The value can contain a maximum of 64 characters.                                                                                                                                                                                                                                                                |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Resource Pool                     | This parameter is optional when you create an enhanced datasource connection. However, you must bind an elastic resource pool to the enhanced datasource connection before using it.                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | The status of the enhanced datasource connection's VPC peering connection is **active**. Used to bind an elastic resource pool or queue that uses a datasource connection.                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |    Before using an enhanced datasource connection, ensure that the created VPC peering connection is in the **Active** state.                                                                                                                                                                                       |
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
      |                                   |       A tag key can contain a maximum of 128 characters. Only letters, digits, spaces, and special characters ``(_.:+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  Tag value: Enter a tag value in the text box.                                                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   |       A tag value can contain a maximum of 255 characters. Only letters, digits, spaces, and special characters ``(_.:+-@)`` are allowed.                                                                                                                                                                           |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   After the creation is complete, the enhanced datasource connection is in the **Active** state, indicating that the connection is successfully created.

(Optional) Step 2: Configure a Routing Rule
-------------------------------------------

Routing rules determine the direction of network traffic by configuring information such as the destination address, next hop type, and next hop address. Routes are classified into system routes and custom routes.

After an enhanced datasource connection is created, the subnet is automatically associated with the system default route. Besides the system default route, you can add custom routing rules as needed to forward traffic destined for the target address to the specified next-hop address.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Datasource Connections**.

#. On the displayed **Enhanced** tab, locate the enhanced datasource connection where you want to add a route and then add a route.

   -  Method 1:

      a. On the **Enhanced** tab, locate the enhanced datasource connection where you want to add a route and click **Manage Route** in its **Operation** column.
      b. On the displayed page, click **Add Route**.
      c. In the **Add Route** dialog box, set the parameters based on :ref:`Table 4 <dli_01_0006__table42440623119>`.
      d. Click **OK**.

   -  Method 2:

      a. On the **Enhanced** tab, locate the enhanced datasource connection where you want to add a route, click **More** in its **Operation** column, and select **Add Route**.
      b. In the **Add Route** dialog box, set the parameters based on :ref:`Table 4 <dli_01_0006__table42440623119>`.
      c. Click **OK**.

   .. _dli_01_0006__table42440623119:

   .. table:: **Table 4** Parameters for adding a custom route

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                              |
      +===================================+==========================================================================================================================================================================================================+
      | Route Name                        | Name of a custom route, which is unique in the same enhanced datasource connection. The name can contain up to 64 characters. Only digits, letters, underscores (_), and hyphens (-) are allowed.        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | IP Address                        | Custom route CIDR block. The CIDR blocks of different routes can overlap but cannot be identical.                                                                                                        |
      |                                   |                                                                                                                                                                                                          |
      |                                   | Do not add the **100.125.**\ *xx.xx* or **100.64.**\ *xx.xx* CIDR blocks to avoid conflicts with the internal CIDR blocks of services like SWR, which can cause enhanced datasource connections to fail. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. After adding a route, you can view the route information on the route details page.

Step 3: Test the Connectivity Between the Queue in the Elastic Resource Pool and the Data Source Address
--------------------------------------------------------------------------------------------------------

#. Log in to the DLI management console. In the navigation pane on the left, choose **Resources** > **Queue Management**.

#. On the **Queue Management** page, locate the row containing the target queue, click **More** in the **Operation** column, and select **Test Address Connectivity**.

#. On the **Test Address Connectivity** page, enter the address to be tested. The domain name and IP address are supported, and the port number can be specified.

   You can input the data source address in the following formats: IPv4 address; IPv4 address + Port number; Domain name; Domain name + Port number.

   -  IPv4 address: 192.168.x.x
   -  IPv4 + Port number: 192.168.x.x:8080
   -  Domain name: domain-xxxxxx.com
   -  Domain name + Port number: domain-xxxxxx.com:8080

#. Click **Test**.

   -  If the test address is reachable, you will receive a message.
   -  If the test address is unreachable, you will also receive a message. Check the network configurations and retry. Network configurations include the VPC peering and the datasource connection. Check whether they have been activated.

.. |image1| image:: /_static/images/en-us_image_0000002363080342.png
