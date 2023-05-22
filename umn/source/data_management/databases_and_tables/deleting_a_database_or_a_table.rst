:original_name: dli_01_0011.html

.. _dli_01_0011:

Deleting a Database or a Table
==============================

You can delete unnecessary databases and tables based on actual conditions.

Precautions
-----------

-  You are not allowed to delete databases or tables that are being used for running jobs.
-  The administrator, database owner, and users with the database deletion permission can delete the database. The administrator, database owner, and users with the table deletion permission can delete the table.

   .. note::

      If a database or table is deleted, it cannot be recovered. Exercise caution when performing this operation.

Deleting a Table
----------------

You can delete a table on either the **Data Management** page or the **SQL Editor** page.

-  Delete the table on the **Data Management** page.

   #. On the left of the management console, choose **Data Management** > **Databases and Tables**.
   #. Locate the row where the database whose tables you want to delete, click the database name to switch to the **Table Management** page.
   #. Locate the row where the target table locates and click **More** > **Delete** in the **Operation** column.
   #. In the displayed dialog box, click **Yes**.

-  Delete a table on the **SQL Editor** page.

   #. On the top menu bar of the DLI management console, click **SQL Editor**.
   #. In the navigation tree on the left, click **Databases**. Click the name of a database where the table you want belongs. The tables of the selected database are displayed.
   #. Click |image1| on the right of the table and choose **Delete** from the shortcut menu.
   #. In the dialog box that is displayed, click **OK**.

Deleting a Database
-------------------

#. On the left of the management console, choose **Data Management** > **Databases and Tables**.
#. Locate the row where the target database locates and click **More** > **Drop Database** in the **Operation** column.

   .. note::

      You cannot delete databases that contain tables. To delete a database containing tables, delete the tables first.

#. In the displayed dialog box, click **Yes**.

.. |image1| image:: /_static/images/en-us_image_0237983632.png
