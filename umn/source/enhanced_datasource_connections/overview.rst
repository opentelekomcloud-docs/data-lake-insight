:original_name: dli_01_0003.html

.. _dli_01_0003:

Overview
========

What Is Enhanced Datasource Connection?
---------------------------------------

Typically, you cannot use DLI to directly access a data source in a VPC other than the one where DLI is because the network between DLI and the data source is disconnected. For proper access, you need to establish a network connection between them.

DLI provides enhanced connections. Establishing a VPC peering connection allows DLI to communicate with the VPC of the data source, supporting cross-source data analysis.

For details about the data sources that support cross-source access, see :ref:`Cross-Source Analysis Development Methods <dli_01_0410>`.

Constraints
-----------

-  Datasource connections cannot be created for the **default** queue.
-  Flink jobs can directly access DIS, OBS, and SMN data sources without using datasource connections.
-  **VPC Administrator** permissions are required for enhanced connections to use VPCs, subnets, routes, VPC peering connections.
-  If you use an enhanced datasource connection, the CIDR block of the elastic resource pool or queue cannot overlap with that of the data source.
-  Only queues bound with datasource connections can access datasource tables.
-  Datasource tables do not support the preview function.
-  When checking the connectivity of datasource connections, the constraints on IP addresses are as follows:

   -  The IP address must be valid, which consists of four decimal numbers separated by periods (.). The value ranges from 0 to 255.

   -  During the test, you can add a port after the IP address and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535.

      For example, **192.168.**\ *xx*\ **.**\ *xx* or **192.168.**\ *xx*\ **.**\ *xx*\ **:8181**.

-  When checking the connectivity of datasource connections, the constraints on domain names are as follows:

   -  The domain name can contain 1 to 255 characters. Only letters, digits, underscores (_), and hyphens (-) are allowed.

   -  The top-level domain name must contain at least two letters, for example, **.com**, **.net**, and **.cn**.

   -  During the test, you can add a port after the domain name and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535.

      For example, **example.com:8080**.

Cross-Source Analysis Process
-----------------------------

To use DLI for cross-source analysis, you need to create a datasource connection to connect DLI to the data source, and then develop jobs to access the data source.


.. figure:: /_static/images/en-us_image_0000001570712116.png
   :alt: **Figure 1** Cross-source analysis flowchart

   **Figure 1** Cross-source analysis flowchart
