:original_name: dli_08_0415.html

.. _dli_08_0415:

Raw
===

Function
--------

The raw format allows you to read and write raw (byte based) values as a single column.

Note: This format encodes null values as **null** of the **byte[]** type. This may have limitation when used in **upsert-kafka**, because **upsert-kafka** treats null values as a tombstone message (DELETE on the key). Therefore, we recommend avoiding using **upsert-kafka** connector and the **raw** format as a **value.format** if the field can have a null value.

The raw format connector is built-in, no additional dependencies are required.

Parameters
----------

.. table:: **Table 1**

   +----------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Default Value | Type   | Description                                                                                                                                             |
   +================+===========+===============+========+=========================================================================================================================================================+
   | format         | Yes       | None          | String | Format to be used. Set this parameter to **raw**.                                                                                                       |
   +----------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | raw.charset    | No        | UTF-8         | String | Charset to encode the text string.                                                                                                                      |
   +----------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | raw.endianness | No        | big-endian    | String | Endianness to encode the bytes of numeric value. Valid values are **big-endian** and **little-endian**. You can search for endianness for more details. |
   +----------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

Supported Connectors
--------------------

-  Kafka
-  UpsertKafka

Example
-------

Use Kafka to send data and output the data to print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and select Flink 1.12. Copy the following statement and submit the job:

   .. code-block::

      create table kafkaSource(
        log string
        ) with (
          'connector' = 'kafka',
          'topic' = '<yourTopic>',
          'properties.group.id' = '<yourGroupId>',
          'properties.bootstrap.servers' = '<yourKafkaAddress>:<yourKafkaPort>',
          'scan.startup.mode' = 'latest-offset',
          'format' = 'raw'
      );
      create table printSink(
        log string
         ) with (
           'connector' = 'print'
         );
      insert into printSink select * from kafkaSource;

#. Insert the following data to the corresponding topic in Kafka:

   .. code-block::

      47.29.201.179 - - [28/Feb/2019:13:17:10 +0000] "GET /?p=1 HTTP/2.0" 200 5316 "https://domain.com/?p=1" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.119 Safari/537.36" "2.75"

#. View the output through either of the following methods:

   -  Method 1: Locate the job and click **More** > **FlinkUI**. Choose **Task Managers** > **Stdout**.
   -  Method 2: If you allow DLI to save job logs in OBS, view the output in the **taskmanager.out** file.

   .. code-block::

      +I(47.29.201.179 - - [28/Feb/2019:13:17:10 +0000] "GET /?p=1 HTTP/2.0"2005316"https://domain.com/?p=1"
      "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.119 Safari/537.36" "2.75")
