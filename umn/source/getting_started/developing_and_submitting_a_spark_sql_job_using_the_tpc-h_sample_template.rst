:original_name: dli_01_0512.html

.. _dli_01_0512:

Developing and Submitting a Spark SQL Job Using the TPC-H Sample Template
=========================================================================

To facilitate SQL operations, DLI allows you to customize query templates or save the SQL statements in use as templates. After templates are saved, you do not need to write SQL statements. You can directly perform the SQL operations using the templates.

The current system provides various standard TPC-H query statement templates. You can select a template as needed. This example shows how to use a TPC-H template to develop and submit a Spark SQL job.

:ref:`Step 1: Log In to the Management Console <dli_01_0512__section19012773105034>`

:ref:`Step 2: Execute the TPC-H Sample Template and View the Result <dli_01_0512__section21590507141153>`

For details about the templates, see SQL Template Management.

.. _dli_01_0512__section19012773105034:

Step 1: Log In to the Management Console
----------------------------------------

#. Go to the DLI console.
#. From the menu on top of the homepage, move your cursor on **Products**.
#. .
#. The DLI management console page is displayed.

   .. note::

      You need to perform authorization when accessing the DLI management console for the first time. For details, see Service Authorization.

.. _dli_01_0512__section21590507141153:

Step 2: Execute the TPC-H Sample Template and View the Result
-------------------------------------------------------------

#. On the DLI management console, choose **Job Templates** > **SQL Templates**, and click the **Sample Templates** tab. Locate the **Q1_Price_summary_report_query** template under **tpchQuery**, and click **Execute** in the **Operation** column. The **SQL Editor** page is displayed.
#. In the upper part of the editing window, set **Engine** to **spark**, **Queues** to **default**, and **Databases** to **default**, and click **Execute**.
#. View the query result in the **View Result** tab in the lower part of the SQL Editor page.

This example uses the **default** queue and database preset in the system as an example. You can also run query statements on a self-created queue and database.

For details about how to create a queue, see "Creating a Queue" in *Data Lake Insight User Guide*. For details about how to create a database, see Creating a Database.
