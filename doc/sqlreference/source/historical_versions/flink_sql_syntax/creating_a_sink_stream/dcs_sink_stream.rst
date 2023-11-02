:original_name: dli_08_0253.html

.. _dli_08_0253:

DCS Sink Stream
===============

Function
--------

DLI exports the Flink job output data to Redis of DCS. Redis is a storage system that supports multiple types of data structures such as key-value. It can be used in scenarios such as caching, event pub/sub, and high-speed queuing. Redis supports direct read/write of strings, hashes, lists, queues, and sets. Redis works with in-memory dataset and provides persistence. For more information about Redis, visit https://redis.io/.

DCS provides Redis-compatible, secure, reliable, out-of-the-box, distributed cache capabilities allowing elastic scaling and convenient management. It meets users' requirements for high concurrency and fast data access.

For more information about DCS, see the *Distributed Cache Service User Guide*.

Prerequisites
-------------

-  Ensure that You have created a Redis cache instance on DCS using your account.

   For details about how to create a Redis cache instance, see **Creating a DCS Instance** in the *Distributed Cache Service User Guide*.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must be interconnected with the DCS clusters. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

-  If you use a VPC peering connection to access a DCS instance, the following restrictions also apply:

   -  If network segment 172.16.0.0/12~24 is used during DCS instance creation, the DLI queue cannot be in any of the following network segments: 192.168.1.0/24, 192.168.2.0/24, and 192.168.3.0/24.
   -  If network segment 192.168.0.0/16~24 is used during DCS instance creation, the DLI queue cannot be in any of the following network segments: 172.31.1.0/24, 172.31.2.0/24, and 172.31.3.0/24.
   -  If network segment 10.0.0.0/8~24 is used during DCS instance creation, the DLI queue cannot be in any of the following network segments: 172.31.1.0/24, 172.31.2.0/24, and 172.31.3.0/24.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "dcs_redis",
       region = "",
       cluster_address = "",
       password = "",
       value_type= "",key_value= ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                        |
   +=======================+=======================+====================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. **dcs_redis** indicates that data is exported to DCS Redis.                                                                                                                                                                                                                   |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region where DCS for storing the data is located.                                                                                                                                                                                                                                                  |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_address       | Yes                   | Redis instance connection address.                                                                                                                                                                                                                                                                 |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password              | No                    | Redis instance connection password. This parameter is not required if password-free access is used.                                                                                                                                                                                                |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value_type            | Yes                   | This parameter can be set to any or the combination of the following options:                                                                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                                                                                                    |
   |                       |                       | -  Data types, including **string**, **list**, **hash**, **set**, and **zset**                                                                                                                                                                                                                     |
   |                       |                       | -  Commands used to set the expiration time of a key, including **expire**, **pexpire**, **expireAt**, and **pexpireAt**                                                                                                                                                                           |
   |                       |                       | -  Commands used to delete a key, including **del** and **hdel**                                                                                                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                                                                                                                    |
   |                       |                       | Use commas (,) to separate multiple commands.                                                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key_value             | Yes                   | Key and value. The number of key_value pairs must be the same as the number of types specified by value_type, and key_value pairs are separated by semicolons (;). Both key and value can be specified through parameter configurations. The dynamic column name is represented by ${column name}. |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.
-  Characters ":", ",", ";", "$", "{", and "}" have been used as special separators without the escape function. These characters cannot be used in key and value as common characters. Otherwise, parsing will be affected and the program exceptions will occur.

Example
-------

Data of stream **qualified_cars** is exported to the Redis cache instance on DCS.

::

   CREATE SINK STREAM qualified_cars (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed DOUBLE,
     total_miles DOUBLE
   )
     WITH (
       type = "dcs_redis",
       cluster_address = "192.168.0.34:6379",
       password = "xxxxxxxx",
       value_type = "string; list; hash; set; zset",
       key_value = "${car_id}_str: ${car_owner}; name_list: ${car_owner}; ${car_id}_hash: {name:${car_owner}, age: ${car_age}}; name_set:   ${car_owner}; math_zset: {${car_owner}:${average_speed}}"
     );
