:original_name: dli_08_0249.html

.. _dli_08_0249:

DDS Sink Stream
===============

Function
--------

DLI outputs the job output data to Document Database Service (DDS).

DDS is compatible with the MongoDB protocol and is secure, highly available, reliable, scalable, and easy to use. It provides DB instance creation, scaling, redundancy, backup, restoration, monitoring, and alarm reporting functions with just a few clicks on the DDS console.

For more information about DDS, see the *Document Database Service User Guide*.

Prerequisites
-------------

-  Ensure that you have created a DDS instance on DDS using your account.

   For details about how to create a DDS instance, see **Buying a DDS DB Instance** in the *Document Database Service Getting Started*.

-  Currently, only cluster instances with SSL authentication disabled are supported. Replica set and single node instances are not supported.

-  In this scenario, jobs must run on the dedicated queue of DLI. Ensure that the dedicated queue of DLI has been created.

-  Ensure that a datasource connection has been set up between the DLI dedicated queue and the DDS cluster, and security group rules have been configured based on the site requirements.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "dds",
       username = "",
       password = "",
       db_url = "",
       field_names = ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory | Description                                                                                                                              |
   +=======================+===========+==========================================================================================================================================+
   | type                  | Yes       | Output channel type. **dds** indicates that data is exported to DDS.                                                                     |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+
   | username              | Yes       | Username for connecting to a database.                                                                                                   |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+
   | password              | Yes       | Password for connecting to a database.                                                                                                   |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+
   | db_url                | Yes       | DDS instance access address, for example, **ip1:port,ip2:port/database/collection**.                                                     |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+
   | field_names           | Yes       | Key of the data field to be inserted. The format is **f1,f2,f3**. Ensure that the key corresponds to the data column in the sink stream. |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+
   | batch_insert_data_num | No        | Amount of data to be written in batches at a time. The value must be a positive integer. The default value is **10**.                    |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Output data in the **qualified_cars** stream to the **collectionTest** DDS DB.

::

   CREATE SINK STREAM qualified_cars (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed INT,
     total_miles INT
   )
     WITH (
       type = "dds",
       region = "xxx",
       db_url = "192.168.0.8:8635,192.168.0.130:8635/dbtest/collectionTest",
       username = "xxxxxxxxxx",
       password =  "xxxxxxxxxx",
       field_names = "car_id,car_owner,car_age,average_speed,total_miles",
       batch_insert_data_num = "10"
     );
