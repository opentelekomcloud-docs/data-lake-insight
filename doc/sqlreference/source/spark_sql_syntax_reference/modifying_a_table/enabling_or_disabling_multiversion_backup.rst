:original_name: dli_08_0354.html

.. _dli_08_0354:

Enabling or Disabling Multiversion Backup
=========================================

Function
--------

DLI controls multiple versions of backup data for restoration. After the multiversion function is enabled, the system automatically backs up table data when you delete or modify the data using **insert overwrite** or **truncate**, and retains the data for a certain period. You can quickly restore data within the retention period. For details about the syntax related to the multiversion function, see :ref:`Backing Up and Restoring Data of Multiple Versions <dli_08_0349>`.

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Syntax
------

-  Enable the multiversion function.

   .. code-block::

      ALTER TABLE [db_name.]table_name
      SET TBLPROPERTIES ("dli.multi.version.enable"="true");

-  Disable the multiversion function.

   ::

      ALTER TABLE [db_name.]table_name
      UNSET TBLPROPERTIES ("dli.multi.version.enable");

   After multiversion is enabled, data of different versions is automatically stored in the OBS storage directory when **insert overwrite** or **truncate** is executed. After multiversion is disabled, run the following statement to restore the multiversion backup data directory:

   .. code-block::

      RESTORE TABLE [db_name.]table_name TO initial layout;

Keyword
-------

-  SET TBLPROPERTIES: Used to set table properties and enable multiversion.
-  UNSET TBLPROPERTIES: Used to unset table properties and disable multiversion.

Parameter
---------

.. table:: **Table 1** Parameter description

   +------------+----------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                      |
   +============+==================================================================================================================================+
   | db_name    | Database name that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_). |
   +------------+----------------------------------------------------------------------------------------------------------------------------------+
   | table_name | Table name                                                                                                                       |
   +------------+----------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Example
-------

-  Modify the **test_table** table to enable multiversion.

   ::

      ALTER TABLE test_table
      SET TBLPROPERTIES ("dli.multi.version.enable"="true");

-  Modify the **test_table** table to disable multiversion.

   ::

      ALTER TABLE test_table
      UNSET TBLPROPERTIES ("dli.multi.version.enable");

   Restore the multiversion backup data directory.

   .. code-block::

      RESTORE TABLE test_table TO initial layout;
