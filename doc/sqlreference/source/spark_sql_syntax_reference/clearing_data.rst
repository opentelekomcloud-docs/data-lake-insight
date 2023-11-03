:original_name: dli_08_0217.html

.. _dli_08_0217:

Clearing Data
=============

Function
--------

This statement is used to delete data from the DLI or OBS table.

Syntax
------

::

   TRUNCATE TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)];

Keyword
-------

.. table:: **Table 1** Parameter

   +-----------+---------------------------------------------------------------------------+
   | Parameter | Description                                                               |
   +===========+===========================================================================+
   | tablename | Name of the target DLI or OBS table that runs the **Truncate** statement. |
   +-----------+---------------------------------------------------------------------------+
   | partcol1  | Partition name of the DLI or OBS table to be deleted.                     |
   +-----------+---------------------------------------------------------------------------+

Precautions
-----------

Only data in the DLI or OBS table can be deleted.

Example
-------

::

   truncate table test PARTITION (class = 'test');
