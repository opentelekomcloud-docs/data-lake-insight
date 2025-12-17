:original_name: dli_01_0003.html

.. _dli_01_0003:

Overview of Enhanced Datasource Connections
===========================================

How to Establish Network Connectivity Between a DLI Elastic Resource Pool and Data Sources?
-------------------------------------------------------------------------------------------

Imagine DLI elastic resource pools and data sources as two isolated islands surrounded by water.

To enable traffic between these two islands, you need to first construct a bridge and then set up signs at both ends of this bridge.

DLI enhanced datasource connections serve as your construction team, enabling you to complete the building of both the bridge and the signs in one go.

By using VPC peering to establish the bridge and defining system routes for direction guidance, you can further enhance navigation with additional custom routes.

If both DLI elastic resource pools and the CIDR block of data sources have IPv6 enabled, you have the option to designate this bridge to utilize the IPv6 network for communication. Moreover, you can create IPv6 routes.


.. figure:: /_static/images/en-us_image_0000002363051176.png
   :alt: **Figure 1** Using an enhanced datasource connection to establish network communication between a DLI elastic resource pool and a data source

   **Figure 1** Using an enhanced datasource connection to establish network communication between a DLI elastic resource pool and a data source

Procedure
---------

This part introduces the network connectivity solution for connecting a DLI elastic resource pool to a data source of a VPC using an enhanced datasource connection.

#. Creating an enhanced datasource connection: Establish a VPC peering connection to connect DLI and the data source's VPC network. For details, see :ref:`Creating an Enhanced Datasource Connection <dli_01_0006>`.

#. Configure routing rules: After an enhanced datasource connection is created, the CIDR block is automatically associated with the system default route without additional operations.

   Besides the system default route, you can add custom routing rules as needed to forward traffic destined for the target address to the specified next-hop address. For details, see :ref:`Adding a Route for an Enhanced Datasource Connection <dli_01_0014>`.

#. Testing network connectivity: Verify the connectivity between the queue and the data source's network. See :ref:`Testing the Network Connectivity Between a Queue and a Data Source <dli_01_0489>`.


.. figure:: /_static/images/en-us_image_0000002397300825.png
   :alt: **Figure 2** Using an enhanced datasource connection to establish network communication between a DLI elastic resource pool and a data source

   **Figure 2** Using an enhanced datasource connection to establish network communication between a DLI elastic resource pool and a data source

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
