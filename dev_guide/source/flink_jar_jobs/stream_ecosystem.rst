:original_name: dli_09_0162.html

.. _dli_09_0162:

Stream Ecosystem
================

Overview
--------

Built on Flink and Spark, the stream ecosystem is fully compatible with the open-source Flink, Storm, and Spark APIs. It is enhanced in features and improved in performance to provide easy-to-use DLI with low latency and high throughput.

The DLI stream ecosystem includes the cloud service ecosystem, open source ecosystem, and custom stream ecosystem.

-  Cloud service ecosystems

   DLI can be interconnected with other services by using Stream SQLs. You can directly use SQL statements to read and write data from various cloud services, such as Data Ingestion Service (DIS), Object Storage Service (OBS), CloudTable Service (CloudTable), MapReduce Service (MRS), Relational Database Service (RDS), Simple Message Notification (SMN), and Distributed Cache Service (DCS).

-  Open-source ecosystems

   After connections to other VPCs are established through VPC peering connections, you can access all data sources and output targets (such as Kafka, HBase, and Elasticsearch) supported by Flink and Spark in your dedicated DLI queues.

-  Custom stream ecosystems

   You can compile code to obtain data from the desired cloud ecosystem or open-source ecosystem as the input data of Flink jobs.

Supported Data Formats
----------------------

DLI Flink jobs support the following data formats:

Avro, Avro_merge, BLOB, CSV, EMAIL, JSON, ORC, Parquet, and XML.
