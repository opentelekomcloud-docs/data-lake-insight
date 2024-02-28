:original_name: dli_spark_max_pt.html

.. _dli_spark_max_pt:

max_pt
======

This function is used to return the name of the largest level-1 partition that contains data in a partitioned table and read the data of this partition.

Syntax
------

.. code-block::

   max_pt(<table_full_name>)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------+--------+-----------------------------------------------------------------------+
   | Parameter       | Mandatory | Type   | Description                                                           |
   +=================+===========+========+=======================================================================+
   | table_full_name | Yes       | STRING | Specified table name. You must have the read permission on the table. |
   +-----------------+-----------+--------+-----------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  The value of the largest level-1 partition is returned.
   -  If a partition is added to a table using the **ALTER TABLE** command, but it does not contain any data, it will not be included in the returned values.

Example Code
------------

For example, table1 is a partitioned table with partitions of **20120801** and **20120802**, both of which contain data, and the **max_pt** value in the following statement will be **20120802**. The DLI SQL statement will read data from the partition with pt = 20120802.

An example command is as follows:

.. code-block::

   select * from tablel where pt = max_pt('dbname.table1');

It is equivalent to the following statement:

.. code-block::

   select * from table1 where pt = (select max(pt) from dbname.table1);
