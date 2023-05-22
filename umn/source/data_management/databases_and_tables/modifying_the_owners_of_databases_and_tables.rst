:original_name: dli_01_0376.html

.. _dli_01_0376:

Modifying the Owners of Databases and Tables
============================================

During actual use, developers create databases and tables and submit them to test personnel for testing. After the test is complete, the databases and tables are transferred to O&M personnel for user experience. In this case, you can change the owner of the databases and tables to transfer data to other owners.

Modifying the Database Owner
----------------------------

You can change the owner of a database on either the **Data Management** page or the **SQL Editor** page.

-  On the **Data Management** page, change the database owner.

   #. On the left of the management console, choose **Data Management** > **Databases and Tables**.
   #. On the **Databases and Tables** page, locate the database you want and click **More** > **Modify Database** in the **Operation** column.
   #. In the displayed dialog box, enter a new owner name (an existing username) and click **OK**.

-  Change the database owner on the **SQL Editor** page.

   #. On the left of the management console, click **SQL Editor**.
   #. In the navigation tree on the left, click **Databases**, click |image1| on the right of the database you want to modify, and choose **Modify Database** from the shortcut menu.
   #. In the displayed dialog box, enter a new owner name (an existing username) and click **OK**.

Modifying the Table Owner
-------------------------

#. On the left of the management console, choose **Data Management** > **Databases and Tables**.
#. Click the name of the database corresponding to the table to be modified. The **Manage Tables** page of the database is displayed.
#. In the **Operation** column of the target table, choose **More** > **Modify Owner**.
#. In the displayed dialog box, enter a new owner name (an existing username) and click **OK**.

.. |image1| image:: /_static/images/en-us_image_0237984360.png
