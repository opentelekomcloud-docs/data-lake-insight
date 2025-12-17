:original_name: dli_01_0011.html

.. _dli_01_0011:

Deleting a Database on the DLI Console
======================================

You can delete an unused database from the DLI console: when a database is no longer needed, such as after a test database has completed testing; if a database has errors or anomalies that cannot be fixed; when there is a need to reorganize the data structure, such as by modifying table designs; or if a database is idle and has no practical use.

This section describes how to delete a database on the DLI management console.

Precautions
-----------

-  Databases or tables that are being used for running jobs cannot be deleted.
-  The administrator, database owner, and users with the database deletion permission can delete the database.

   .. note::

      If a database or table is deleted, it cannot be recovered. Exercise caution when performing this operation.

Deleting a Database
-------------------

#. In the navigation pane on the left of the console, choose **Data Management** > **Databases and Tables**.
#. Locate the row where the target database locates and click **More** > **Drop Database** in the **Operation** column.

   .. note::

      You cannot delete databases that contain tables. To delete a database containing tables, delete the tables first.

#. In the displayed dialog box, click **Yes**.
