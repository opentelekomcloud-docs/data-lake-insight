:original_name: dli_01_0639.html

.. _dli_01_0639:

Developing and Submitting a SQL Job Using a SQL Job Template
============================================================

DLI allows you to create custom templates or save currently used SQL statements as templates for quick and convenient SQL operations. Once a template is saved, you can execute SQL operations directly through the template without the need to write SQL statements.

The system offers sample templates that include various standard TPC-H query statements. You can choose to use one of these templates or create a custom template to create a SQL job.

This example shows how to use a TPC-H sample template to develop and submit a SQL job.

Procedure
---------

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Job Templates** > **SQL Templates**.
#. On the displayed **Sample Templates** tab, find a sample template that matches your service scenario under **tpchQuery** and click **Execute** in the **Operation** column.
#. In the upper part of the editing window, set **Engine** to **Spark**, **Queues** to **default**, and **Databases** to **default**, and click **Execute**.
#. Check the query result on the **View Result** tab below the editing window.

This example uses the **default** queue and database preset in the system as an example. You can also run the command in a self-created queue and database.

For details, see "Creating a Queue" in the *Data Lake Insight User Guide*. For how to create a database, see "Data Management" > "Databases and Tables" > "Creating a Database" in the *Data Lake Insight User Guide*.
