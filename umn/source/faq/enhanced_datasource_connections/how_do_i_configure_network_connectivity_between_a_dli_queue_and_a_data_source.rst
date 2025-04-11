:original_name: dli_03_0186.html

.. _dli_03_0186:

How Do I Configure Network Connectivity Between a DLI Queue and a Data Source?
==============================================================================

-  **Configuring the Connection Between a DLI Queue and a Data Source in a Private Network**

   If your DLI job needs to connect to a data source, for example, MRS, RDS, CSS, Kafka, or GaussDB(DWS), you need to enable the network between DLI and the data source.

   An enhanced datasource connection uses VPC peering to directly connect the VPC networks of the desired data sources for point-to-point data exchanges.


   .. figure:: /_static/images/en-us_image_0000001391378486.png
      :alt: **Figure 1** Configuration process

      **Figure 1** Configuration process

-  **Configuring the Connection Between a DLI Queue and a Data Source in the Internet**

   You can configure SNAT rules and add routes to the public network to enable communications between a queue and the Internet.


   .. figure:: /_static/images/en-us_image_0000001441378549.png
      :alt: **Figure 2** Configuration process

      **Figure 2** Configuration process
