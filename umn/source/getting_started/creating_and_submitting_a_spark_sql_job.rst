:original_name: dli_01_0002.html

.. _dli_01_0002:

Creating and Submitting a Spark SQL Job
=======================================

Scenarios
---------

DLI can query data stored in OBS. This section describes how to us a Spark SQL job on DLI to query OBS data.

Procedure
---------

You can use DLI to submit a Spark SQL job to query data. The general procedure is as follows:

:ref:`Step 1: Upload Data to OBS <dli_01_0002__section61379418181550>`

:ref:`Step 2: Create a Queue <dli_01_0002__section10742144985011>`

:ref:`Step 3: Create a Database <dli_01_0002__section21433273112656>`

:ref:`Step 4: Create a Table <dli_01_0002__section21590507141153>`

:ref:`Step 5: Query Data <dli_01_0002__section37788816112733>`

.. _dli_01_0002__section61379418181550:

Step 1: Upload Data to OBS
--------------------------

Before you use DLI to query and analyze data, upload data files to OBS.

#. Go to the DLI console.

#. In the service list, click **Object Storage Service** under **Storage**. The OBS console page is displayed.

#. Create a bucket. In this example, the bucket name is **obs1**.

   a. Click **Create Bucket** in the upper right corner.
   b. On the displayed **Create Bucket** page, enter the **Bucket Name**. Retain the default values for other parameters or set them as required.

      .. note::

         You must select the same region as the DLI management console.

   c. Click **Create Now**.

#. Click **obs1** to switch to the **Overview** page.

#. In the left navigation pane, click **Objects**. Click **Upload Object**. In the displayed dialog box, drag files or folders to upload or add file, for example, **sampledata.csv** to the file upload box. Then, click **Upload**.

   You can create a **sampledata.txt** file, copy the following content separated by commas (,), and save the file as **sampledata.csv**.

   .. code-block::

      12,test

   After the file is uploaded successfully, the file path is **obs://obs1/sampledata.csv**.

   .. note::

      -  For more information about OBS operations, see the *Object Storage Service Console Operation Guide*.
      -  For more information about the tool, see the *OBS Tool Guide*.
      -  You are advised to use an OBS tool, such as OBS Browser+, to upload large files because OBS Console has restrictions on the file size and quantity.

         -  OBS Browser+ is a graphical tool that provides complete functions for managing your buckets and objects in OBS.

.. _dli_01_0002__section10742144985011:

Step 2: Create a Queue
----------------------

A queue is the basis for using DLI. Before executing an SQL job, you need to create a queue.

-  An available queue **default** is preset in DLI.
-  You can also create queues as needed.

   #. Log in to the DLI management console.

   #. On the DLI management console, click **SQL Editor** in the navigation pane on the left. The **SQL Editor** page is displayed.

   #. On the left pane, select the **Queues** tab, and click |image1| next to **Queues**.

      For details, see Creating a Queue.

.. _dli_01_0002__section21433273112656:

Step 3: Create a Database
-------------------------

Before querying data, create a database, for example, **db1**.

.. note::

   The **default** database is a built-in database. You cannot create the **default**. database.

#. On the DLI management console, click **SQL Editor** in the navigation pane on the left. The **SQL Editor** page is displayed.

#. In the editing window on the right of the **SQL Editor** page, enter the following SQL statement and click **Execute**. Read and agree to the privacy agreement, and click **OK**.

   .. code-block::

      create database db1;

   After database **db1** is successfully created, **db1** will be displayed in the **Database** list.

   .. note::

      When you execute a query on the DLI management console for the first time, you need to read the privacy agreement. You can perform operations only after you agree to the agreement. For later queries, you will not need to read the privacy agreement again.

.. _dli_01_0002__section21590507141153:

Step 4: Create a Table
----------------------

After database **db1** is created, create a table (for example, **table1**) containing data in the sample file **obs://obs1/sampledata.csv** stored on OBS in **db1**.

#. In the SQL editing window of the **SQL Editor** page, select the **default** queue and database **db1**.

#. Enter the following SQL statement in the job editor window and click **Execute**:

   .. code-block::

      create table table1 (id int, name string) using csv options (path 'obs://obs1/sampledata.csv');

   After the table is created, click the **Databases** tab and then select **db1**. The created table **table1** is displayed in the **Table** area.

.. _dli_01_0002__section37788816112733:

Step 5: Query Data
------------------

After performing the preceding steps, you can start querying data.

#. In the **Table** tab on the **SQL Editor** page, double-click the created table **table1**. The SQL statement is automatically displayed in the SQL job editing window in the right pane. Run following statement to query 1,000 records in the **table1** table:

   .. code-block::

      select * from db1.table1 limit 1000;

#. Click **Execute**. The system starts the query.

   After the SQL statement is executed successfully, you can view the query result in **View Result** under the SQL job editing window.

.. |image1| image:: /_static/images/en-us_image_0276441461.png
