:original_name: dli_08_0353.html

.. _dli_08_0353:

Configuring the Trash Bin for Expired Multiversion Data
=======================================================

Function
--------

After the multiversion function is enabled, expired backup data will be directly deleted by the system when the **insert overwrite** or **truncate** statement is executed. You can configure the trash bin of the OBS parallel file system to accelerate the deletion of expired backup data. To enable the trash bin, add **dli.multi.version.trash.dir** to the table properties. For details about the syntax for enabling or disabling the multiversion function, see :ref:`Enabling or Disabling Multiversion Backup <dli_08_0354>`.

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Syntax
------

::

   ALTER TABLE [db_name.]table_name
   SET TBLPROPERTIES ("dli.multi.version.trash.dir"="OBS bucket for expired multiversion backup data");

Keywords
--------

-  TBLPROPERTIES: This keyword is used to add a **key/value** property to a table.

Parameters
----------

.. table:: **Table 1** Parameters

   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                       | Description                                                                                                                                                                                                                                                                                                                                     |
   +=================================================+=================================================================================================================================================================================================================================================================================================================================================+
   | db_name                                         | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_).                                                                                                                                                                                            |
   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                                      | Table name                                                                                                                                                                                                                                                                                                                                      |
   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OBS bucket for expired multiversion backup data | A directory in the bucket where the current OBS table locates. You can change the directory path as needed. For example, if the current OBS table directory is **obs://bucketName/filePath** and a **Trash** directory has been created in the OBS table directory, you can set the trash bin directory to **obs://bucketName/filePath/Trash**. |
   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.
-  To automatically empty the trash bin, you need to configure a lifecycle rule for the bucket of the OBS parallel file system. The procedure is as follows:

   #. On the OBS console, choose **Parallel File System** in the left navigation pane. Click the name of the target file system. The **Overview** page is displayed.
   #. In the left navigation pane, choose **Basic Configurations** > **Lifecycle Rules** to create a lifecycle rule.

Example
-------

Configure the trash bin to accelerate the deletion of expired backup data. The data is dumped to the **/.Trash** directory in OBS.

::

   ALTER TABLE test_table
   SET TBLPROPERTIES ("dli.multi.version.trash.dir"="/.Trash");
