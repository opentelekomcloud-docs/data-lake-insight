:original_name: dli_03_0128.html

.. _dli_03_0128:

Why Is Creating a VPC Peering Connection Necessary for Enhanced Datasource Connections in DLI?
==============================================================================================

The main reason for creating a VPC peering connection for DLI's enhanced datasource connection is to establish network connectivity between DLI and data sources in different VPCs.

When DLI needs to access external data sources located in different VPCs, it cannot directly read the data due to network isolation. By creating an enhanced datasource connection, a VPC peering connection can be used to bridge the VPC networks of DLI and the data sources, enabling data exchange and cross-source analysis.

Advantages of enhanced datasource connections:

-  Network connectivity: Directly connect DLI to the target data source's VPC network for data exchange.
-  Support for multiple data sources: Connect DLI to multiple data sources, such as GaussDB(DWS), RDS, CSS, and DCS.
