:original_name: dli_03_0162.html

.. _dli_03_0162:

How Do I Change the Field Names of an Existing Table on DLI?
============================================================

DLI does not support directly changing the field names of a table. However, you can solve this issue by migrating the table data using the following steps:

#. Create a table: Create a table and define new field names.
#. Migrate the data: Use the **INSERT INTO ... SELECT** statement to migrate the data from the old table to the new table.
#. Delete the old table: Once you have ensured that the new table has completely replaced the old table and the data migration is complete, you can delete the old table to avoid confusion.
