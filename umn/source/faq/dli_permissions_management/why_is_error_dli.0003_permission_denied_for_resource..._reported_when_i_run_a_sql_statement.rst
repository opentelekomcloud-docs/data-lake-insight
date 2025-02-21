:original_name: dli_03_0227.html

.. _dli_03_0227:

Why Is Error "DLI.0003: Permission denied for resource..." Reported When I Run a SQL Statement?
===============================================================================================

Symptom
-------

When the SQL query statement is executed, the system displays a message indicating that the user does not have the permission to query resources.

Error information: **DLI.0003: Permission denied for resource 'databases.dli_test.tables.test.columns.col1', User = '{UserName}', Action = 'SELECT'**

Solution
--------

The user does not have the permission to query the table.

In the navigation pane on the left of the DLI console page, choose **Data Management** > **Databases and Tables**, search for the desired database table, view the permission configuration, and grant the table query permission to the user who requires it.
