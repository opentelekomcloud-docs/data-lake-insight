:original_name: dli_03_0215.html

.. _dli_03_0215:

How Do I Do If Renaming a Table After a SQL Job Causes Incorrect Data Size?
===========================================================================

Changing a table name immediately after executing a SQL job may result in an incorrect data size for the table.

This is because DLI updates the metadata of the table when executing a SQL job. If the table name is changed before the job is completed, it conflicts with the metadata update process of the job, which affects the determination of the data size.

To avoid this situation, you are advised to wait for 5 minutes after the SQL job is executed before changing the table name. This ensures that the system has enough time to update the metadata of the table, avoiding inaccurate data size statistics caused by changing the table name.
