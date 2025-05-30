:original_name: dli_01_0003.html

.. _dli_01_0003:

Overview of Enhanced Datasource Connections
===========================================

Why Create Enhanced Datasource Connections?
-------------------------------------------

In cross-source data analysis scenarios, DLI needs to connect to external data sources. However, due to the different VPCs between the data source and DLI, the network cannot be connected, which results in DLI being unable to read data from the data source. DLI's enhanced datasource connection feature enables network connectivity between DLI and the data source.

This section will introduce a solution for cross-VPC data source network connectivity:

-  Creating an enhanced datasource connection: Establish a VPC peering connection to connect DLI and the data source's VPC network.
-  Testing network connectivity: Verify the connectivity between the queue and the data source's network.

For details about the data sources that support cross-source access, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

.. caution::

   In cross-source development scenarios, there is a risk of password leakage if datasource authentication information is directly configured. You are advised to use Data Encryption Workshop (DEW) to store authentication information of data sources when Spark 3.3.1 or later and Flink 1.15 or later jobs access data sources using datasource connections. This will help you address issues related to data security, key security, and complex key management. For details, see :ref:`Using DEW to Manage Access Credentials for Data Sources <dli_01_0636>`.

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

Cross-Source Analysis Process
-----------------------------

To use DLI for cross-source analysis, you need to create a datasource connection to connect DLI to the data source, and then develop jobs to access the data source.


.. figure:: /_static/images/en-us_image_0000001570712116.png
   :alt: **Figure 1** Cross-source analysis flowchart

   **Figure 1** Cross-source analysis flowchart
