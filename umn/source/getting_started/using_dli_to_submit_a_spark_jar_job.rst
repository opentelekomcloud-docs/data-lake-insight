:original_name: dli_01_0375.html

.. _dli_01_0375:

Using DLI to Submit a Spark Jar Job
===================================

Scenario
--------

DLI allows you to submit Spark jobs compiled as JAR files, which contain the necessary code and dependency information for executing the job. These files are used for specific data processing tasks such as data query, analysis, and machine learning. Before submitting a Spark Jar job, upload the package to OBS and submit it along with the data and job parameters to run the job.

This example introduces the basic process of submitting a Spark Jar job package through the DLI console. Due to different service requirements, the specific writing of the Jar package may vary. It is recommended that you refer to the sample code provided by DLI and edit and customize it according to your actual business scenario.

Procedure
---------

:ref:`Table 1 <dli_01_0375__table1478217572316>` describes the procedure for submitting a Spark Jar job using DLI.

.. _dli_01_0375__table1478217572316:

.. table:: **Table 1** Procedure for submitting a Spark Jar job using DLI

   +---------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
   | Step                                                                                                          | Description                                                         |
   +===============================================================================================================+=====================================================================+
   | :ref:`Step 1: Upload Data to OBS <dli_01_0375__section10891114913473>`                                        | Prepare a Spark Jar job package and upload it to OBS.               |
   +---------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
   | :ref:`Step 2: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0375__section1573781010172>` | Create compute resources required for submitting the Spark Jar job. |
   +---------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
   | :ref:`Step 3: Submit a Spark Job <dli_01_0375__section21590507141153>`                                        | Create a Spark Jar job to analyze data.                             |
   +---------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+

.. _dli_01_0375__section10891114913473:

Step 1: Upload Data to OBS
--------------------------

Develop a Spark Jar job program, compile it, and pack it into **spark-examples.jar**. Perform the following steps to upload the program:

Before submitting Spark Jar jobs, upload data files to OBS.

#. Log in to the DLI console.

#. In the service list, click **Object Storage Service** under **Storage**.

#. Create a bucket. In this example, name it **dli-test-obs01**.

   a. On the displayed **Buckets** page, click **Create Bucket** in the upper right corner.
   b. On the displayed **Create Bucket** page, enter the **Bucket Name**. Retain the default values for other parameters or set them as required.

      .. note::

         Select a region that matches the location of the DLI console.

   c. Click **Create Now**.

#. In the bucket list, click the name of the **dli-test-obs01** bucket you just created to access its **Objects** tab.

#. Click **Upload Object**. In the dialog box displayed, drag or add files or folders, for example, **spark-examples.jar**, to the upload area. Then, click **Upload**.

   In this example, the path after upload is **obs://dli-test-obs01/spark-examples.jar**.

   For more operations on the OBS console, see the *Object Storage Service User Guide*.

.. _dli_01_0375__section1573781010172:

Step 2: Create an Elastic Resource Pool and Add Queues to the Pool
------------------------------------------------------------------

In this example, the elastic resource pool **dli_resource_pool** and queue **dli_queue_01** are created.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed page, click **Buy Resource Pool** in the upper right corner.

#. On the displayed page, set the parameters.

   :ref:`Table 2 <dli_01_0375__dli_01_0002_table67098261452>` describes the parameters.

   .. _dli_01_0375__dli_01_0002_table67098261452:

   .. table:: **Table 2** Parameters

      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Parameter          | Description                                                                                                                                                                                             | Example Value     |
      +====================+=========================================================================================================================================================================================================+===================+
      | Region             | Select a region where you want to buy the elastic resource pool.                                                                                                                                        | \_                |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Project            | Project uniquely preset by the system for each region                                                                                                                                                   | Default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Name               | Name of the elastic resource pool                                                                                                                                                                       | dli_resource_pool |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Specifications     | Specifications of the elastic resource pool                                                                                                                                                             | Standard          |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | CU Range           | The maximum and minimum CUs allowed for the elastic resource pool                                                                                                                                       | 64-64             |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | CIDR Block         | CIDR block the elastic resource pool belongs to. If you use an enhanced datasource connection, this CIDR block cannot overlap that of the data source. **Once set, this CIDR block cannot be changed.** | 172.16.0.0/19     |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Enterprise Project | Select an enterprise project for the elastic resource pool.                                                                                                                                             | default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+

#. Click **Buy**.

#. Click **Submit**.

#. In the elastic resource pool list, locate the pool you just created and click **Add Queue** in the **Operation** column.

#. Set the basic parameters listed below.

   .. table:: **Table 3** Basic parameters for adding a queue

      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                        | Example Value         |
      +=======================+====================================================================+=======================+
      | Name                  | Name of the queue to add                                           | dli_queue_01          |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Type                  | Type of the queue                                                  | \_                    |
      |                       |                                                                    |                       |
      |                       | -  To execute SQL jobs, select **For SQL**.                        |                       |
      |                       | -  To execute Flink or Spark jobs, select **For general purpose**. |                       |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Enterprise Project    | Select an enterprise project.                                      | default               |
      +-----------------------+--------------------------------------------------------------------+-----------------------+

#. Click **Next** and configure scaling policies for the queue.

   Click **Create** to add a scaling policy with varying priority, period, minimum CUs, and maximum CUs.

   .. table:: **Table 4** Scaling policy parameters

      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                                          | Example Value         |
      +=======================+======================================================================================================================================================================================================================+=======================+
      | Priority              | Priority of the scaling policy in the current elastic resource pool. A larger value indicates a higher priority. In this example, only one scaling policy is configured, so its priority is set to **1** by default. | 1                     |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Period                | The first scaling policy is the default policy, and its **Period** parameter configuration cannot be deleted or modified.                                                                                            | 00-24                 |
      |                       |                                                                                                                                                                                                                      |                       |
      |                       | The period for the scaling policy is from 00 to 24.                                                                                                                                                                  |                       |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Min CU                | Minimum number of CUs allowed by the scaling policy                                                                                                                                                                  | 16                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Max CU                | Maximum number of CUs allowed by the scaling policy                                                                                                                                                                  | 64                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

.. _dli_01_0375__section21590507141153:

Step 3: Submit a Spark Job
--------------------------

#. On the DLI management console, choose **Job Management** > **Spark Jobs** in the navigation pane on the left. On the displayed page, click **Create Job** in the upper right corner.

#. Set the following Spark job parameters:

   -  **Queue**: Select the queue created in :ref:`Step 2: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0375__section1573781010172>`.
   -  **Spark Version**: Select a Spark engine version.
   -  **Application**: Select the package created in :ref:`Step 1: Upload Data to OBS <dli_01_0375__section10891114913473>`.

   For other parameters, refer to the description about the Spark job editing page in "Creating a Spark Job" in the *Data Lake Insight User Guide*.

#. Click **Execute** in the upper right corner of the Spark job editing window, read and agree to the privacy agreement, and click **OK**. Submit the job. A message is displayed, indicating that the job is submitted.

#. (Optional) Switch to the **Job Management > Spark Jobs** page to view the status and logs of the submitted Spark job.
