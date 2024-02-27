:original_name: dli_08_0260.html

.. _dli_08_0260:

Creating a Redis Table
======================

Create a Redis table to connect to the source stream.

For details about the JOIN syntax, see :ref:`JOIN Between Stream Data and Table Data <dli_08_0106>`.

Syntax
------

::

   CREATE TABLE table_id (key_attr_name STRING(, hash_key_attr_name STRING)?, value_attr_name STRING)
     WITH (
       type = "dcs_redis",
       cluster_address = ""(,password = "")?,
       value_type= "",
       key_column= ""(,hash_key_column="")?);

Keywords
--------

.. table:: **Table 1** Keywords

   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory | Description                                                                                                                                                                                   |
   +=================+===========+===============================================================================================================================================================================================+
   | type            | Yes       | Output channel type. Value **dcs_redis** indicates that data is exported to DCS Redis.                                                                                                        |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_address | Yes       | Redis instance connection address.                                                                                                                                                            |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password        | No        | Redis instance connection password. This parameter is not required if password-free access is used.                                                                                           |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value_type      | Yes       | Indicates the field data type. Supported data types include string, list, hash, set, and zset.                                                                                                |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key_column      | Yes       | Indicates the column name of the Redis key attribute.                                                                                                                                         |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hash_key_column | No        | If **value_type** is set to **hash**, this field must be specified as the column name of the level-2 key attribute.                                                                           |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cache_max_num   | No        | Indicates the maximum number of cached query results. The default value is **32768**.                                                                                                         |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cache_time      | No        | Indicates the maximum duration for caching database query results in the memory. The unit is millisecond. The default value is **10000**. The value **0** indicates that caching is disabled. |
   +-----------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  Redis clusters are not supported.

-  Ensure that You have created a Redis cache instance on DCS using your account.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with DCS instance. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Example
-------

The Redis table is used to connect to the source stream.

.. code-block::

   CREATE TABLE table_a (attr1 string, attr2 string, attr3 string)
     WITH (
       type = "dcs_redis",
       value_type = "hash",
       key_column = "attr1",
       hash_key_column = "attr2",
       cluster_address = "192.168.1.238:6379",
       password = "xxxxxxxx"
    );
