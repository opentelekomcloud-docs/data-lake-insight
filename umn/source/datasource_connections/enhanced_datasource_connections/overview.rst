:original_name: dli_01_0003.html

.. _dli_01_0003:

Overview
========

The enhanced datasource connection uses VPC peering at the bottom layer to directly connect the VPC network between the DLI cluster and the destination datasource. Data is exchanged in point-to-point mode. The enhanced datasource connection function supports all cross-source services implemented by DLI, including CloudTable HBase, CloudTableOpenTSDB, MRS OpenTSDB, DWS, RDS, CSS, DCS, and DDS. In addition, UDFs, Spark jobs, and Flink jobs can be used to access self-built data sources.

.. note::

   -  The CIDR block of the DLI queue bound with a datasource connection cannot overlap with that of the data source.
   -  Datasource connections cannot be created for the default queue.
   -  To access a datasource connection table, you need to use the queue for which a datasource connection has been created.
   -  The preview function is not supported for datasource tables.

The enhanced datasource scenario provides the following functions:

-  :ref:`Creating, Querying, and Deleting an Enhanced Datasource Connection <dli_01_0006>`
-  :ref:`Binding and Unbinding a Queue <dli_01_0009>`
-  :ref:`Modifying Host Information <dli_01_0013__section636281512389>`
-  :ref:`Custom Route Information <dli_01_0014>`
-  :ref:`Enhanced Datasource Connection Permission Management <dli_01_0018>`

Enhanced Datasource Connection Page
-----------------------------------

This page displays all enhanced datasource connections. If there are a large number of connections, they are displayed on multiple pages.

.. table:: **Table 1** Datasource connection list parameters

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                 |
   +===================================+=============================================================================================================================================================+
   | Connection Name                   | Name of the created datasource connection.                                                                                                                  |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Connection Status                 | Status of a datasource connection. Currently, the console displays only connections in the **Active** state.                                                |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | VPC                               | VPC used by the destination data source.                                                                                                                    |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Subnet                            | Subnet used by the destination data source.                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Created                           | Time when a connection is created. The connections in the connection list can be displayed according to the creation time in ascending or descending order. |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Delete Connection**: Delete a created enhanced datasource connection.                                                                                  |
   |                                   | -  **Manage Route**: Used to display the custom route information of the enhanced datasource connection.                                                    |
   |                                   | -  **More**:                                                                                                                                                |
   |                                   |                                                                                                                                                             |
   |                                   |    -  **Modify Host**: Customize the IP address corresponding to the host or domain name.                                                                   |
   |                                   |    -  **Bind Queue**: Bind a queue to an enhanced datasource connection.                                                                                    |
   |                                   |    -  **Unbind Queue**: Unbind an enhanced datasource connection from a queue.                                                                              |
   |                                   |    -  **Add Route**: Add a custom route for the enhanced datasource connection.                                                                             |
   |                                   |    -  **Delete Route**: Delete a custom route for an enhanced datasource connection.                                                                        |
   |                                   |    -  **Manage Permissions**: Authorize or reclaim permissions for other projects.                                                                          |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
