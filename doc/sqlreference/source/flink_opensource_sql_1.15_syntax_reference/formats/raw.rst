:original_name: dli_08_15026.html

.. _dli_08_15026:

Raw
===

Function
--------

The Raw format allows to read and write raw (byte based) values as a single column.

.. note::

   -  This format encodes null values as null of byte[] type. This may have limitation when used in **upsert-kafka**, because **upsert-kafka** treats null values as a tombstone message (DELETE on the key). Therefore, we recommend avoiding using **upsert-kafka** connector and the **raw** format as a **value.format** if the field can have a null value.
   -  The raw format connector is built-in, no additional dependencies are required. For details, see `Raw Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/raw/>`__.

Supported Connectors
--------------------

-  Kafka
-  Upsert Kafka
-  FileSystem

Parameter Description
---------------------

.. table:: **Table 1**

   +----------------+-----------+---------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Default Value | Type   | Description                                                                                                                                                                                            |
   +================+===========+===============+========+========================================================================================================================================================================================================+
   | format         | Yes       | None          | String | Format to be used. Set this parameter to **raw**.                                                                                                                                                      |
   +----------------+-----------+---------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | raw.charset    | No        | UTF-8         | String | Charset to encode the text string.                                                                                                                                                                     |
   +----------------+-----------+---------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | raw.endianness | No        | big-endian    | String | Endianness to encode the bytes of numeric value. Valid values are **big-endian** and **little-endian**. You can search for `endianness <https://en.wikipedia.org/wiki/Endianness>`__ for more details. |
   +----------------+-----------+---------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

The table below details the SQL types the format supports, including details of the serializer and deserializer class for encoding and decoding.

.. table:: **Table 2** Data type mapping

   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Flink SQL Type             | Value                                                                                                                          |
   +============================+================================================================================================================================+
   | CHAR/VARCHAR/STRING        | A UTF-8 (by default) encoded text string. The encoding charset can be configured by **raw.charse**.                            |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | BINARY / VARBINARY / BYTES | The sequence of bytes itself.                                                                                                  |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | BOOLEAN                    | A single byte to indicate boolean value, **0** means **false**, **1** means **true**.                                          |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | TINYINT                    | A single byte of the signed number value.                                                                                      |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | SMALLINT                   | Two bytes with big-endian (by default) encoding. The endianness can be configured by **raw.endianness**.                       |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | INT                        | Four bytes with big-endian (by default) encoding. The endianness can be configured by **raw.endianness**.                      |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | BIGINT                     | Eight bytes with big-endian (by default) encoding. The endianness can be configured by **raw.endianness**.                     |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | FLOAT                      | Four bytes with IEEE 754 format and big-endian (by default) encoding. The endianness can be configured by **raw.endianness**.  |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | DOUBLE                     | Eight bytes with IEEE 754 format and big-endian (by default) encoding. The endianness can be configured by **raw.endianness**. |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | RAW                        | The sequence of bytes serialized by the underlying TypeSerializer of the RAW type.                                             |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Use Kafka to send data and output the data to Print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and select Flink 1.15. Copy the following statement and submit the job:

   .. code-block::

      CREATE TABLE kafkaSource (
        log string
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'kafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'raw'
      );

      CREATE TABLE printSink (
        log string
      ) WITH (
        'connector' = 'print'
      );
      insert into printSink select * from kafkaSource;

#. Insert the following data to the corresponding topic in Kafka:

   .. code-block::

      47.29.201.179 - - [28/Feb/2019:13:17:10 +0000] "GET /?p=1 HTTP/2.0" 200 5316 "https://domain.com/?p=1" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.119 Safari/537.36" "2.75"

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      +I[47.29.201.179 - - [28/Feb/2019:13:17:10 +0000] "GET /?p=1 HTTP/2.0" 200 5316 "https://domain.com/?p=1" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.119 Safari/537.36" "2.75"]
