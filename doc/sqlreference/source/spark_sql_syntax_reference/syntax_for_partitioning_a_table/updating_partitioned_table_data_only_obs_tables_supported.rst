:original_name: dli_08_0079.html

.. _dli_08_0079:

Updating Partitioned Table Data (Only OBS Tables Supported)
===========================================================

Function
--------

This statement is used to update the partition information about a table in the Metastore.

Syntax
------

::

   MSCK REPAIR TABLE table_name;

Or

.. code-block::

   ALTER TABLE table_name RECOVER PARTITIONS;

Keywords
--------

-  PARTITIONS: partition information
-  SERDEPROPERTIES: Serde attribute

Parameters
----------

.. table:: **Table 1** Parameters

   =============== ================
   Parameter       Description
   =============== ================
   table_name      Table name
   partition_specs Partition fields
   obs_path        OBS path
   =============== ================

Precautions
-----------

-  This statement is applied only to partitioned tables. After you manually add partition directories to OBS, run this statement to update the newly added partition information in the metastore. The **SHOW PARTITIONS table_name** statement can be used to query the newly-added partitions.
-  The partition directory name must be in the specified format, that is, **tablepath/partition_column_name=partition_column_value**.

Example
-------

Run the following statements to update the partition information about table **ptable** in the Metastore:

::

   MSCK REPAIR TABLE ptable;

Or

.. code-block::

   ALTER TABLE ptable RECOVER PARTITIONS;
