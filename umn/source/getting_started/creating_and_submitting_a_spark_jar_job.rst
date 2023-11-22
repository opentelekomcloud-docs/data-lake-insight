:original_name: dli_01_0375.html

.. _dli_01_0375:

Creating and Submitting a Spark Jar Job
=======================================

Scenario
--------

DLI can query data stored in OBS. This section describes how to use a Spark Jar job on DLI to query OBS data in real time.

Procedure
---------

You can use DLI to submit Spark jobs for real-time computing. The general procedure is as follows:

:ref:`Step 1: Upload Data to OBS <dli_01_0375__section10891114913473>`

:ref:`Step 2: Create a Queue <dli_01_0375__section122981023152710>`

:ref:`Step 3: Create a Package <dli_01_0375__section21433273112656>`

:ref:`Step 4: Submit a Spark Job <dli_01_0375__section21590507141153>`

.. _dli_01_0375__section10891114913473:

Step 1: Upload Data to OBS
--------------------------

Write a Spark Jar job program, and compile and pack it as **spark-examples.jar**. Perform the following steps to submit the job:

Before submitting Spark Jar jobs, upload data files to OBS.

#. Log in to the DLI console.

#. In the service list, click **Object Storage Service** under **Storage**. The OBS console page is displayed.

#. Create a bucket. In this example, name it **dli-test-obs01**.

   a. Click **Create Bucket**.
   b. On the displayed **Create Bucket** page, enter the **Bucket Name**. Retain the default values for other parameters or set them as required.

      .. note::

         When creating an OBS bucket, you must select the same region as the DLI management console.

   c. Click **Create Now**.

#. Click **dli-test-obs01** to switch to the **Objects** tab page.

#. Click **Upload Object**. In the dialog box displayed, drag or add files or folders, for example, **spark-examples.jar**, to the upload area. Then, click **Upload**.

   After the file is uploaded successfully, the file path is **obs://dli-test-obs01/spark-examples.jar**.

   .. note::

      -  For more information about OBS operations, see the *Object Storage Service Console Operation Guide*.
      -  For more information about the tool, see the *OBS Tool Guide*.
      -  You are advised to use an OBS tool, such as OBS Browser+, to upload large files because OBS Console has restrictions on the file size and quantity.

         -  OBS Browser+ is a graphical tool that provides complete functions for managing your buckets and objects in OBS. You are advised to use this tool to create buckets or upload objects.

.. _dli_01_0375__section122981023152710:

Step 2: Create a Queue
----------------------

If you submit a Spark job for the first time, you need to create a queue first. For example, create a queue named **sparktest** and set **Queue Type** to **General Queue**.

#. Log in to the DLI management console.
#. In the navigation pane of the DLI management console, choose **Resources** > **Queue Management**.
#. In the upper right corner of the **Queue Management** page, click **Create Queue** to create a queue.
#. Create a queue, name it **sparktest**, and set the queue usage to for general purpose. For details, see Creating a Queue.
#. Click **Create Now** to create a queue.

.. _dli_01_0375__section21433273112656:

Step 3: Create a Package
------------------------

Before submitting a Spark job, you need to create a package, for example, **spark-examples.jar**.

#. In the navigation pane on the left of the DLI console, choose **Data Management** > **Package Management**.

#. On the **Package Management** page, click **Create** in the upper right corner to create a package.

#. In the **Create Package** dialog box, set **Type** to **JAR**, **OBS Path** to the path of the spark-examples.jar package in :ref:`Step 1: Upload Data to OBS <dli_01_0375__section10891114913473>`, and **Group** to **Do not use**.

#. Click **OK**.

   You can view and select the package on the **Package Management** page.

For details about how to create a package, see "Creating a Package".

.. _dli_01_0375__section21590507141153:

Step 4: Submit a Spark Job
--------------------------

#. On the DLI management console, choose **Job Management > Spark Jobs** in the navigation pane on the left. On the displayed page, click **Create Job**.

#. On the Spark job editing page, set **Queues** to the queue created in :ref:`Step 2: Create a Queue <dli_01_0375__section122981023152710>` and **Application** to the package created in :ref:`Step 3: Create a Package <dli_01_0375__section21433273112656>`.

   For details about other parameters, see the description of the Spark job editing page in "Creating a Spark Job".

#. Click **Execute** in the upper right corner of the Spark job editing window, read and agree to the privacy agreement, and click **OK**. Submit the job. A message is displayed, indicating that the job is submitted.

#. (Optional) Switch to the **Job Management > Spark Jobs** page to view the status and logs of the submitted Spark job.

   .. note::

      When you click **Execute** on the DLI management console for the first time, you need to read the privacy agreement. Once agreed to the agreement, you will not receive any privacy agreement messages for subsequent operations.
