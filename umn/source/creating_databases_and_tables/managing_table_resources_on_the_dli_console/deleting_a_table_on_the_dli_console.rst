:original_name: dli_01_0626.html

.. _dli_01_0626:

Deleting a Table on the DLI Console
===================================

You can delete tables as needed.

Precautions
-----------

-  Databases or tables that are being used for running jobs cannot be deleted.
-  Only administrators, table owners, and users with table deletion permission can delete tables.

   .. note::

      Deleted data tables cannot be restored. Exercise caution when performing this operation.

Deleting a Table
----------------

You can delete a table on either the **Data Management** page or the **SQL Editor** page.

-  On the **Data Management** page

   #. In the navigation pane on the left of the console, choose **Data Management** > **Databases and Tables**.
   #. Locate the database whose tables you want to delete and click its name.
   #. On the displayed page, select the table you want to delete, click **More** in the **Operation** column, and select **Drop Table**.
   #. In the displayed dialog box, click **Yes**.

-  On the **SQL Editor** page

   #. In the navigation pane on the left of the console, choose **SQL Editor**.
   #. In the middle pane, click the **Databases** tab. Click the name of the database whose tables you want to delete.
   #. Click |image1| next to the table to delete and select **Delete**.
   #. In the **Drop Table** dialog box that appears, click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001994614536.png
