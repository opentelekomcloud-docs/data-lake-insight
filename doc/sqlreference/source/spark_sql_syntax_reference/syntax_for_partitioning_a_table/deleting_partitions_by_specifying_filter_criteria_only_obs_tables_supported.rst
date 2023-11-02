:original_name: dli_08_0343.html

.. _dli_08_0343:

Deleting Partitions by Specifying Filter Criteria (Only OBS Tables Supported)
=============================================================================

Function
--------

This statement is used to delete one or more partitions based on specified conditions.

Precautions
-----------

-  **This statement is used for OBS table operations only.**
-  The table in which partitions are to be deleted must exist. Otherwise, an error is reported.
-  The to-be-deleted partition must exist. Otherwise, an error is reported. To avoid this error, add **IF EXISTS** in this statement.

Syntax
------

::

   ALTER TABLE [db_name.]table_name
     DROP [IF EXISTS]
     PARTITIONS partition_filtercondition;

Keyword
-------

-  DROP: deletes specified partitions.
-  IF EXISTS: Partitions to be deleted must exist. Otherwise, an error is reported.
-  PARTITIONS: specifies partitions meeting the conditions

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                 |
   +===================================+=============================================================================================================================================================================================================================================================================================+
   | db_name                           | Database name that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_).                                                                                                                                                            |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                        | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | **This statement is used for OBS table operations.**                                                                                                                                                                                                                                        |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_filtercondition         | Condition used to search partitions to be deleted. The format is as follows:                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | -  *Partition column name* :ref:`Operator <dli_08_0061__en-us_topic_0093946932_t34b3b699258a401085f3c3b3ad1a3717>` *Value to compare*                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   |    Example: start_date < '201911'                                                                                                                                                                                                                                                           |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | -  <partition_filtercondition1> AND|OR <partition_filtercondition2>                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   |    Example: start_date < '201911' OR start_date >= '202006'                                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | -  (<partition_filtercondition1>) [,partitions (<partition_filtercondition2>), ...]                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   |    Example: (start_date <> '202007'), partitions(start_date < '201912')                                                                                                                                                                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

You can run the following statements to delete partitions of the **student** table using different conditions:

::

   alter table student drop partitions(start_date < '201911');
   alter table student drop partitions(start_date >= '202007');
   alter table student drop partitions(start_date BETWEEN '202001' AND '202007');
   alter table student drop partitions(start_date < '201912' OR start_date >= '202006');
   alter table student drop partitions(start_date > '201912' AND start_date <= '202004');
   alter table student drop partitions(start_date != '202007');
   alter table student drop partitions(start_date <> '202007');
   alter table student drop partitions(start_date <> '202007'), partitions(start_date < '201912');
