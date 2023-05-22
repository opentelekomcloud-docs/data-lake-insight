:original_name: dli_01_0410.html

.. _dli_01_0410:

Datasource Connection and Cross-Source Analysis
===============================================

DLI supports the datasource capability of the native Spark and extends it. With DLI datasource connection, you can access other data storage services through SQL statements, Spark jobs, and Flink jobs and import, query, analyze, and process data in the services.

Datasource Connections
----------------------

Before using DLI to perform cross source analysis, you need to set up a datasource connection to enable the network between data sources.

The enhanced datasource connection uses VPC peering at the bottom layer to directly connect the VPC network between the DLI queue and the destination datasource. Data is exchanged in point-to-point mode.

.. note::

   -  Datasource connections cannot be created for the default queue.
   -  **VPC Administrator** permissions are required to use the VPC, subnet, route, VPC peering connection, and port for DLI datasource connections. You can set this parameter in :ref:`Service Authorization <dli_01_0486>`.

Cross-Source Analysis
---------------------

The enhanced datasource connection supports all cross-source services implemented by DLI and implements access to self-built data sources by using UDFs, Spark jobs, and Flink jobs.

Currently, DLI supports datasource connection to the following data sources: CloudTable HBase, CloudTable OpenTSDB, CSS, DCS Redis, DDS Mongo, DIS, DMS, DWS, MRS HBase, MRS Kafka, MRS OpenTSDB, OBS, RDS MySQL, RDS PostGre, and SMN.

.. note::

   -  To access a datasource connection table, you need to use the queue for which a datasource connection has been created.
   -  The preview function is not supported for datasource tables.

Datasource Connection Analysis Process
--------------------------------------

To use DLI for cross-source analysis, you need to create datasource connections and then develop different jobs to access data sources. Perform the following steps:

#. Create a datasource connection. You can create a connection in either of the following ways:

   -  Create a datasource connection on the management console.
   -  Create a datasource connection through an API.

#. Develop a DLI job to connect to a datasource. You can connect to a datasource in one of the following ways:

   -  Develop a SQL job to connect to a datasource.
   -  Develop a Spark job to connect to a datasource.
   -  Develop a Flink job to connect to a datasource.

The following describes the basic processes of developing SQL jobs, Spark jobs, and Flink jobs for datasource connection.

-  SQL Job


   .. figure:: /_static/images/en-us_image_0264894338.png
      :alt: **Figure 1** Datasource connection analysis process

      **Figure 1** Datasource connection analysis process

-  Spark Job


   .. figure:: /_static/images/en-us_image_0264934285.png
      :alt: **Figure 2** Datasource connection analysis process

      **Figure 2** Datasource connection analysis process

-  Flink Job


   .. figure:: /_static/images/en-us_image_0264975521.png
      :alt: **Figure 3** Datasource connection analysis process

      **Figure 3** Datasource connection analysis process
