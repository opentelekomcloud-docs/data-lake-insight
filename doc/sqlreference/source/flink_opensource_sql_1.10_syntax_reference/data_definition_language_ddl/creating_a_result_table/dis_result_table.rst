:original_name: dli_08_0310.html

.. _dli_08_0310:

DIS Result Table
================

Function
--------

DLI writes the Flink job output data into DIS. The data is filtered and imported to the DIS stream for future processing.

DIS addresses the challenge of transmitting data outside cloud services to cloud services. DIS builds data intake streams for custom applications capable of processing or analyzing streaming data. DIS continuously captures, transmits, and stores terabytes of data from hundreds of thousands of sources every hour, such as logs, Internet of Things (IoT) data, social media feeds, website clickstreams, and location-tracking events. For more information about DIS, see the *Data Ingestion Service User Guide*.

Syntax
------

::

   create table disSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector.type' = 'dis',
     'connector.region' = '',
     'connector.channel' = '',
     'format.type' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory | Description                                                                                                                                           |
   +=========================+===========+=======================================================================================================================================================+
   | connector.type          | Yes       | Data source type. Set this parameter to **dis**.                                                                                                      |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.region        | Yes       | Region where DIS for storing the data locates.                                                                                                        |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.ak            | No        | Access key ID. This parameter must be set in pair with **sk**.                                                                                        |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sk            | No        | Secret access key. This parameter must be set in pair with **ak**.                                                                                    |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.channel       | Yes       | Name of the DIS stream where data is located.                                                                                                         |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type             | Yes       | Data coding format. The value can be **csv** or **json**.                                                                                             |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.field-delimiter  | No        | Attribute delimiter. You can customize the attribute delimiter only when the encoding format is CSV. The default delimiter is a comma (,).            |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.partition-key | No        | Group primary key. Multiple primary keys are separated by commas (,). If this parameter is not specified, data is randomly written to DIS partitions. |
   +-------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Output the data in the **disSink** stream to DIS.

::

   create table disSink(
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_speed INT
   )
   with (
     'connector.type' = 'dis',
     'connector.region' = '',
     'connector.channel' = 'disOutput',
     'connector.partition-key' = 'car_id,car_owner',
     'format.type' = 'csv'
   );
