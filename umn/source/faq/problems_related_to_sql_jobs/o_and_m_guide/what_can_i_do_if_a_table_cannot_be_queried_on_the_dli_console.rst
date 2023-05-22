:original_name: dli_03_0184.html

.. _dli_03_0184:

What Can I Do If a Table Cannot Be Queried on the DLI Console?
==============================================================

Symptom
-------

A DLI table exists but cannot be queried on the DLI console.

Possible Causes
---------------

If a table exists but cannot be queried, there is a high probability that the current user does not have the permission to query or operate the table.

Solution
--------

Contact the user who creates the table and obtain the required permissions. To assign permissions, perform the following steps:

#. Log in to the DLI management console as the user who creates the table. Choose **Data Management** > **Databases and Tables** form the navigation pane on the left.
#. Click the database name. The table management page is displayed. In the **Operation** column of the target table, click **Permissions**. The table permission management page is displayed.
#. Click **Set Permission**. In the displayed dialog box, set **Authorization Object** to **User**, set **Username** to the name of the user that requires the permission, and select the required permissions. For example, **Select Table** and **Insert** permissions.
#. Click **OK**.
#. Log in to the DLI console as the user that has been granted permission and check whether the table can be queried.
