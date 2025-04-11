:original_name: dli_08_0106.html

.. _dli_08_0106:

JOIN Between Stream Data and Table Data
=======================================

The JOIN operation allows you to query data from a table and write the query result to the sink stream. Currently, only RDSs and DCS Redis tables are supported. The ON keyword describes the Key used for data query and then writes the **Value** field to the sink stream.

For details about the data definition statements of RDS tables, see :ref:`Creating an RDS Table <dli_08_0261>`.

For details about the data definition statements of Redis tables, see :ref:`Creating a Redis Table <dli_08_0260>`.

Syntax
------

::

   FROM tableExpression JOIN tableExpression
     ON value11 = value21 [ AND value12 = value22]

Syntax Description
------------------

The ON keyword only supports equivalent query of table attributes. If level-2 keys exist (specifically, the Redis value type is HASH), the AND keyword needs to be used to express the equivalent query between Key and Hash Key.

Precautions
-----------

None

Example
-------

Perform equivalent JOIN between the vehicle information source stream and the vehicle price table, get the vehicle price data, and write the price data into the vehicle information sink stream.

::

   CREATE SOURCE STREAM car_infos (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_detail_type STRING
   )
   WITH (
     type = "dis",
     region = "",
     channel = "dliinput",
     partition_count = "1",
     encode = "csv",
     field_delimiter = ","
   );

   /** Create a data dimension table to connect to the source stream to fulfill field backfill.
     *
     * Reconfigure the following options according to actual conditions:
     * value_type: indicates the value type of the Redis key value. The value can be STRING, HASH, SET, ZSET, or LIST. For the HASH type, you need to specify hash_key_column as the layer-2 primary key. For the SET type, you need to concatenate all queried values using commas (,).
     * key_column: indicates the column name corresponding to the primary key of the dimension table.
     * hash_key_column: indicates the column name that corresponds to the KEY in the HASHMAP when value_type is set to HASH. If value_type is not HASH, you do not need to set this option.
     * cluster_address: indicates the DCS Redis cluster address.
     * password: indicates the DCS Redis cluster password.
     **/
   CREATE TABLE car_price_table (
     car_brand STRING,
     car_detail_type STRING,
     car_price STRING
   )
   WITH (
     type = "dcs_redis",
     value_type = "hash",
     key_column = "car_brand",
     hash_key_column = "car_detail_type",
     cluster_address = "192.168.1.238:6379",
     password = "xxxxxxxx"
   );

   CREATE SINK STREAM audi_car_owner_info (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_detail_type STRING,
     car_price STRING
   )
   WITH (
     type = "dis",
     region = "",
     channel = "dlioutput",
     partition_key = "car_owner",
     encode = "csv",
     field_delimiter = ","
   );

   INSERT INTO audi_car_owner_info
   SELECT t1.car_id, t1.car_owner, t2.car_brand, t1.car_detail_type, t2.car_price
   FROM car_infos as t1 join car_price_table as t2
   ON t2.car_brand = t1.car_brand and t2.car_detail_type = t1.car_detail_type
   WHERE t1.car_brand = "audi";
