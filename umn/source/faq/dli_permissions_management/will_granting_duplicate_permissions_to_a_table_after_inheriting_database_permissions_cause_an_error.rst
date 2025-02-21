:original_name: dli_03_0057.html

.. _dli_03_0057:

Will Granting Duplicate Permissions to a Table After Inheriting Database Permissions Cause an Error?
====================================================================================================

If a table inherits database permissions, you do not need to regrant the inherited permissions to the table.

Re-authorizing may cause confusion in table permission management, as the inherited permissions are already sufficient.

When you grant permissions on a table on the console:

-  If you set **Authorization Object** to **User** and select the permissions that are the same as the inherited permissions, the system displays a message indicating that the permissions already exist and do not need to be regranted.
-  If you set **Authorization Object** to **Project** and select the permissions that are the same as the inherited permissions, the system does not notify you of duplicate permissions.
