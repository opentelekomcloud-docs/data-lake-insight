:original_name: dli_08_0091.html

.. _dli_08_0091:

Viewing Table Creation Statements
=================================

Function
--------

This statement is used to show the statements for creating a table.

Syntax
------

::

   SHOW CREATE TABLE table_name;

Keyword
-------

CREATE TABLE: statement for creating a table

Parameters
----------

.. table:: **Table 1** Parameter description

   ========== ===========
   Parameter  Description
   ========== ===========
   table_name Table name
   ========== ===========

Precautions
-----------

The table specified in this statement must exist. Otherwise, an error will occur.

Example
-------

#. Create a table. For details, see :ref:`Creating an OBS Table <dli_08_0223>` or :ref:`Creating a DLI Table <dli_08_0224>`.

#. Run the following statement to view the statement that is used to create table **test**:

   ::

      SHOW CREATE TABLE test;
