:original_name: dli_08_0302.html

.. _dli_08_0302:

DIS Source Table
================

Function
--------

Create a source stream to read data from DIS. DIS accesses user data and Flink job reads data from the DIS stream as input data for jobs. Flink jobs can quickly remove data from producers using DIS source sources for continuous processing. Flink jobs are applicable to scenarios where data outside the cloud service is imported to the cloud service for filtering, real-time analysis, monitoring reports, and dumping.

DIS addresses the challenge of transmitting data outside cloud services to cloud services. DIS builds data intake streams for custom applications capable of processing or analyzing streaming data. DIS continuously captures, transmits, and stores terabytes of data from hundreds of thousands of sources every hour, such as logs, Internet of Things (IoT) data, social media feeds, website clickstreams, and location-tracking events. For more information about DIS, see the *Data Ingestion Service User Guide*.

Syntax
------

.. code-block::

   create table disSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   )
   with (
     'connector.type' = 'dis',
     'connector.region' = '',
     'connector.channel' = '',
     'format-type' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                      | Mandatory             | Description                                                                                                                                                                                        |
   +================================+=======================+====================================================================================================================================================================================================+
   | connector.type                 | Yes                   | Data source type. Set this parameter to **dis**.                                                                                                                                                   |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.region               | Yes                   | Region where DIS for storing the data locates.                                                                                                                                                     |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.ak                   | No                    | Access key ID. This parameter must be set in pair with **sk**.                                                                                                                                     |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sk                   | No                    | Secret access key. This parameter must be set in pair with **ak**.                                                                                                                                 |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.channel              | Yes                   | Name of the DIS stream where data is located.                                                                                                                                                      |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.partition-count      | No                    | Number of partitions where data will be read. Data in partition 0 to **partition-count** will be read.                                                                                             |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | Neither this parameter or **partition-range** can be configured.                                                                                                                                   |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | If neither of the two parameters is set, all partition data will be read by default.                                                                                                               |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.partition-range      | No                    | Range of partitions where data will be read. Neither this parameter or **partition-count** can be configured. If neither of the two parameters is set, all partition data will be read by default. |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | For example, if you set **partition-range** to **[0:2]**, data in partitions 1, 2, and 3 will be read. The range must be within the DIS stream.                                                    |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.offset               | No                    | Start position from which data will be read. Either this parameter or **start-time** can be configured.                                                                                            |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.start-time           | No                    | Time from which DLI reads data                                                                                                                                                                     |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | If this parameter is specified, DLI reads data read from the specified time. The format is **yyyy-MM-dd HH:mm:ss**.                                                                                |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | If neither **start-time** nor **offset** is specified, the latest data is read.                                                                                                                    |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector. enable-checkpoint   | No                    | Whether to enable the checkpoint function. The value can be **true** (enabled) or **false** (disabled). The default value is **false**.                                                            |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | Do not set this parameter when **offset** or **start-time** is set. If this parameter is set to **true**, **checkpoint-app-name** must be configured.                                              |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector. checkpoint-app-name | No                    | ID of a DIS consumer. If a DIS stream is consumed by different jobs, you need to configure the consumer ID for each job to avoid checkpoint confusion.                                             |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | Do not set this parameter when **offset** or **start-time** is set. If **checkpoint-app-name** is set to **true**, this parameter is mandatory.                                                    |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector. checkpoint-interval | No                    | Interval of checkpoint operations on the DIS source operator. The default value is **60s**. Available value units: d, day/h, hour/min, minute/s, sec, second                                       |
   |                                |                       |                                                                                                                                                                                                    |
   |                                |                       | Do not set this parameter when **offset** or **start-time** is configured.                                                                                                                         |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type                    | Yes                   | Data coding format. The value can be **csv** or **json**.                                                                                                                                          |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.field-delimiter         | No                    | Attribute delimiter. You can customize the attribute delimiter only when the encoding format is CSV. The default delimiter is a comma (,).                                                         |
   +--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

::

   create table disCsvSource (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed INT,
     total_miles INT)
   with (
     'connector.type' = 'dis',
     'connector.region' = '',
     'connector.channel' = 'disInput',
     'format.type' = 'csv'
   );
