:original_name: dli_03_0057.html

.. _dli_03_0057:

Will an Error Be Reported if the Inherited Permissions Are Regranted to a Table That Inherits Database Permissions?
===================================================================================================================

If a table inherits database permissions, you do not need to regrant the inherited permissions to the table.

When you grant permissions on a table on the console:

-  If you set **Authorization Object** to **User** and select the permissions that are the same as the inherited permissions, the system displays a message indicating that the permissions already exist and do not need to be regranted.
-  If you set **Authorization Object** to **Project** and select the permissions that are the same as the inherited permissions, the system does not notify you of duplicate permissions.
