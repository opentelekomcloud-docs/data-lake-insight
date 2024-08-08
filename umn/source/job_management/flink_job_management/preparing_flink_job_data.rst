:original_name: dli_01_0454.html

.. _dli_01_0454:

Preparing Flink Job Data
========================

To create a Flink job, you need to enter the data source and data output channel, that is, source and sink. To use another service as the source or sink stream, you need to apply for the service first.

Flink jobs support the following data sources and output channels:

-  DIS as the data input and output channel

   To use DIS as the data source and output channel, you need to enable DIS first.

   For details about how to create a DIS stream, see **Creating a DIS Stream** in the *Data Ingestion Service User Guide*.

   After applying for a DIS stream, you can upload local data to DIS to provide data sources for Flink jobs in real time. For details, see **Sending Data to DIS** in the *Data Ingestion Service User Guide*.

   An example is provided as follows:

   .. code-block::

      1,lilei,bmw320i,28
      2,hanmeimei,audia4,27

-  OBS as the data source

   To use OBS as the data source, enable OBS first. For details about how to enable OBS, see **Enabling OBS** in the *Object Storage Service Console Operation Guide*.

   After you enable OBS, upload local files to OBS using the Internet. For detailed operations, see **Uploading a File** in the *Object Storage Service Console Operation Guide*.

-  RDS as the output channel

   To use RDS as the output channel, create an RDS instance. For details, see **Creating a DB Instance** in the *Relational Database Service User Guide*.

-  SMN as the output channel

   To use SMN as the output channel, create an SMN topic to obtain the URN resource ID and then add topic subscription. For detailed operations, see **Getting Started** in the *Simple Message Notification User Guide*.

-  Kafka as the data input and output channel

   If Kafka serves as both the source and sink streams, create an enhanced datasource connection between Flink jobs and Kafka. For details, see :ref:`Enhanced Datasource Connections <dli_01_0426>`.

   If the port of the Kafka server is listened on by the host name, you need to add the mapping between the host name and IP address of the Kafka Broker node to the datasource connection.

-  CloudTable as the data input and output channel

   To use CloudTable as the data input and output channel, create a cluster in CloudTable and obtain the cluster ID.

-  CSS as the output channel

   To use CSS as the data output channel, create a cluster in CSS and obtain the cluster's private network address. For details, see **Getting Started** in the *Cloud Search Service User Guide*.

-  DCS as the output channel

   To use DCS as the output channel, create a Redis cache instance in DCS and obtain the address used for Flink jobs to connect to the Redis instance. For details, see "Buying a DCS Redis Instance" in *Distributed Cache Service User Guide*.
