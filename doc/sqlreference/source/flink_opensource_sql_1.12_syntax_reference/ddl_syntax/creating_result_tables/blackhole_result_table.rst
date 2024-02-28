:original_name: dli_08_0392.html

.. _dli_08_0392:

BlackHole Result Table
======================

Function
--------

The BlackHole connector allows for swallowing all input records. It is designed for high-performance testing and UDF output. It is not a substantive sink. The BlackHole result table is a built-in connector.

For example, if an error is reported when you register a result table of another type, but you are not sure whether it is caused by a system fault or an invalid setting of the **WITH** parameter for the result table, you can change the value of **connector** to **blackhole** and click **Run**. If no error is reported, the system is normal. You must check the settings of the **WITH** parameter.

Prerequisites
-------------

None

Precautions
-----------

When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

Syntax
------

.. code-block::

   create table blackhole_table (
    attr_name attr_type (',' attr_name attr_type) *
   ) with (
    'connector' = 'blackhole'
   );

Parameters
----------

.. table:: **Table 1**

   +-----------+-----------+---------------+-----------+------------------------------------------------------------+
   | Parameter | Mandatory | Default Value | Data Type | Description                                                |
   +===========+===========+===============+===========+============================================================+
   | connector | Yes       | None          | String    | Connector to be used. Set this parameter to **blackhole**. |
   +-----------+-----------+---------------+-----------+------------------------------------------------------------+

Example
-------

The DataGen source table generates data, and the BlackHole result table receives the data.

.. code-block::

   create table datagenSource (
    user_id string,
    user_name string,
    user_age int
   ) with (
    'connector' = 'datagen',
    'rows-per-second'='1'
   );
   create table blackholeSink (
    user_id string,
    user_name string,
    user_age int
   ) with (
    'connector' = 'blackhole'
   );
   insert into blackholeSink select * from datagenSource;
