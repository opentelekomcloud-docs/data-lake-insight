:original_name: dli_03_0175.html

.. _dli_03_0175:

Do I Need to Grant Table Permissions to a User and Project After I Delete a Table and Create One with the Same Name?
====================================================================================================================

Scenario
--------

User A created the **testTable** table in a database through a SQL job and granted user B the permission to insert and delete table data. User A deleted the **testTable** table and created a new **testTable** table. If user A wants user B to retain the insert and delete permission, user A needs to grant the permissions to user B again.

Possible Causes
---------------

After a table is deleted, the table permissions are not retained. You need to grant permissions to a user or project.

Solution
--------

Operations to grant permissions to a user or project are as follows:

#. On the left of the management console, choose **Data Management** > **Databases and Tables**.
#. Click the database name whose table permission is to be granted. The table management page of the database is displayed.
#. Locate the row of the target table and click **Permissions** in the **Operation** column.
#. On the displayed page, click **Grant Permission** in the upper right corner.
#. In the displayed **Grant Permission** dialog box, select the required permissions.
#. Click **OK**.
