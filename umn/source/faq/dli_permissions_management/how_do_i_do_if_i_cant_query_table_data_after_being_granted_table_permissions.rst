:original_name: dli_03_0228.html

.. _dli_03_0228:

How Do I Do If I Can't Query Table Data After Being Granted Table Permissions?
==============================================================================

If you have already granted authorization to a table and a test query was successful, but you encounter an error when trying to query it again after some time, you should first check if the current table permissions still exist:

-  Check if the permissions still exist:

   If your permissions have been revoked, it may result in a missing permission error when trying to query the table data.

-  Check the table creation time:

   Check if the table has been deleted and recreated by others. A table with the same name as a deleted table does not inherit the permissions of the deleted table and is not considered the same table.
