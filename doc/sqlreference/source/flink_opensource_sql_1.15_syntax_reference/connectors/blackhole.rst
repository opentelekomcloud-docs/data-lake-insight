:original_name: dli_08_15029.html

.. _dli_08_15029:

BlackHole
=========

Function
--------

The BlackHole connector allows for swallowing all input records. It is designed for high-performance testing and UDF output. It is not a substantive sink. The BlackHole result table is a built-in connector.

For example, if an error is reported when you register a result table of another type, but you are not sure whether it is caused by a system fault or an invalid setting of the **WITH** parameter for the result table, you can change the value of **connector** to **blackhole** and click **Run**. If no error is reported, the system is normal. You must check the settings of the **WITH** parameter.

.. table:: **Table 1** Supported types

   ===================== ============
   Type                  Description
   ===================== ============
   Supported Table Types Result table
   ===================== ============

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

Syntax
------

.. code-block::

   create table blackhole_table (
    attr_name attr_type (',' attr_name attr_type) *
   ) with (
    'connector' = 'blackhole'
   );

Parameter Description
---------------------

.. table:: **Table 2** Parameter

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
