:original_name: dli_01_0512.html

.. _dli_01_0512:

Submitting a Flink Jar Job Using DLI
====================================

Scenario
--------

Flink Jar jobs are suitable for data analysis scenarios that require custom stream processing logic, complex state management, or integration with specific libraries. You need to write and build a Jar job package. Before submitting a Flink Jar job, upload the Jar job package to OBS and submit it together with the data and job parameters to run the job.

This example introduces the basic process of submitting a Flink Jar job package through the DLI console. Due to different service requirements, the specific writing of the Jar package may vary. It is recommended that you refer to the sample code provided by DLI and edit and customize it according to your actual business scenario.

Procedure
---------

:ref:`Table 1 <dli_01_0512__table1478217572316>` describes the procedure for submitting a Flink Jar job using DLI.

.. _dli_01_0512__table1478217572316:

.. table:: **Table 1** Procedure for submitting a Flink Jar job using DLI

   +---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | Step                                                                                                                      | Description                                                                               |
   +===========================================================================================================================+===========================================================================================+
   | :ref:`Step 1: Develop a JAR File and Upload It to OBS <dli_01_0512__section31930062913>`                                  | Prepare a Flink Jar job package and upload it to OBS.                                     |
   +---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | :ref:`Step 2: Buy an Elastic Resource Pool and Create Queues Within It <dli_01_0512__section199641553174919>`             | Create compute resources required for submitting the Flink Jar job.                       |
   +---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | :ref:`Step 3: Use DEW to Manage Access Credentials <dli_01_0512__section1144135816224>`                                   | In cross-source analysis scenarios, use DEW to manage access credentials of data sources. |
   +---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | :ref:`Step 4: Create a Custom Agency to Allow DLI to Access DEW and Read Credentials <dli_01_0512__section1253291911573>` | Create an agency to allow DLI to access DEW.                                              |
   +---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | :ref:`Step 5: Create a Flink Jar Job and Configure Job Information <dli_01_0512__section131564016912>`                    | Create a Flink Jar job to analyze data.                                                   |
   +---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+

.. _dli_01_0512__section31930062913:

Step 1: Develop a JAR File and Upload It to OBS
-----------------------------------------------

Develop a JAR file offline as the DLI console does not support this capability. For development examples, refer to "Flink Jar Job Examples".

Develop a Flink Jar job program, compile it, and pack it into **flink-examples.jar**. Perform the following steps to upload the program:

Before submitting a Flink job, upload data files to OBS.

#. Log in to the management console.

#. In the service list, click **Object Storage Service** under **Storage**.

#. Create a bucket. In this example, name it **dli-test-obs01**.

   a. On the displayed **Buckets** page, click **Create Bucket** in the upper right corner.
   b. On the displayed **Create Bucket** page, set **Bucket Name**. Retain the default values for other parameters or modify them as needed.

      .. note::

         Select a region that matches the location of the DLI console.

   c. Click **Create Now**.

#. In the bucket list, click the name of the **dli-test-obs01** bucket you just created to access its **Objects** tab.

#. Click **Upload Object**. In the displayed dialog box, drag or add files or folders, for example, **flink-examples.jar**, to the upload area. Then, click **Upload**.

   In this example, the path after upload is **obs://dli-test-obs01/flink-examples.jar**.

   For more operations on the OBS console, see *Object Storage Service User Guide*.

.. _dli_01_0512__section199641553174919:

Step 2: Buy an Elastic Resource Pool and Create Queues Within It
----------------------------------------------------------------

When running a Flink job in datasource scenarios, you cannot use the existing **default** queue. You need to create a general-purpose queue. In this example, the elastic resource pool **dli_resource_pool** and queue **dli_queue_01** are created.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed page, click **Buy Resource Pool** in the upper right corner.

#. On the displayed page, set the parameters.

   :ref:`Table 2 <dli_01_0512__dli_01_0002_table67098261452>` describes the parameters.

   .. _dli_01_0512__dli_01_0002_table67098261452:

   .. table:: **Table 2** Parameter descriptions

      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Parameter          | Description                                                                                                                                                                                             | Example Value     |
      +====================+=========================================================================================================================================================================================================+===================+
      | Region             | Select a region where you want to buy the elastic resource pool.                                                                                                                                        | \_                |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Project            | Project uniquely preset by the system for each region                                                                                                                                                   | Default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Name               | Name of the elastic resource pool                                                                                                                                                                       | dli_resource_pool |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Type               | Specifications of the elastic resource pool                                                                                                                                                             | Standard          |
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
      | Type                  | Type of the queue                                                  | For general purpose   |
      |                       |                                                                    |                       |
      |                       | -  To execute SQL jobs, select **For SQL**.                        |                       |
      |                       | -  To execute Flink or Spark jobs, select **For general purpose**. |                       |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Enterprise Project    | Select an enterprise project for the elastic resource pool.        | default               |
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
      | Max CU                | Maximum number of CUs allowed by the scaling policy.                                                                                                                                                                 | 64                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

.. _dli_01_0512__section1144135816224:

Step 3: Use DEW to Manage Access Credentials
--------------------------------------------

In cross-source analysis scenarios, you need to set attributes such as the username and password in the connector. However, information such as usernames and passwords is highly sensitive and needs to be encrypted to ensure user data privacy.

DEW offers a secure, reliable, and easy-to-use solution for encrypting and decrypting sensitive data.

This example introduces how to create a shared secret in DEW.

#. Log in to the DEW management console.

#. In the navigation pane on the left, choose **Cloud Secret Management Service** > **Secrets**.

#. Click **Create Secret**. On the displayed page, configure basic secret information.

   -  In this example, the key in the first line is the user's access key ID (AK).
   -  In this example, the key in the second line is the user's secret access key (SK).

#. Set access credential parameters on the DLI Flink Jar job editing page.

   .. code-block::

      flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key=USER_AK_CSMS_KEY_obstest1
      flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key=USER_SK_CSMS_KEY_obstest1
      flink.hadoop.fs.obs.security.provider=com.dli.provider.UserObsBasicCredentialProvider
      flink.hadoop.fs.dew.csms.secretName=obsAksK
      flink.hadoop.fs.dew.endpoint=kmsendpoint
      flink.hadoop.fs.dew.csms.version=v6
      flink.hadoop.fs.dew.csms.cache.time.second=3600
      flink.dli.job.agency.name=agencyname

.. _dli_01_0512__section1253291911573:

Step 4: Create a Custom Agency to Allow DLI to Access DEW and Read Credentials
------------------------------------------------------------------------------

#. Log in to the management console.

#. In the upper right corner of the page, hover over the username and select **Identity and Access Management**.

#. In the navigation pane of the IAM console, choose **Agencies**.

#. On the displayed page, click **Create Agency**.

#. On the **Create Agency** page, set the following parameters:

   -  **Agency Name**: Enter an agency name, for example, **dli_dew_agency_access**.
   -  **Agency Type**: Select **Cloud service**.
   -  **Cloud Service**: This parameter is available only when you select **Cloud service** for **Agency Type**. Select **Data Lake Insight (DLI)** from the drop-down list.
   -  **Validity Period**: Select **Unlimited**.
   -  **Description**: You can enter **Agency with OBS OperateAccess permissions**. This parameter is optional.

#. Click **Next**.

#. Click the agency name. On the displayed page, click the **Permissions** tab. Click **Authorize**. On the displayed page, click **Create Policy**.

#. .. _dli_01_0512__en-us_topic_0118645629_li1671455941710:

   Configure policy information.

   a. Enter a policy name, for example, **dli-dew-agency**.

   b. Select **JSON**.

   c. In the **Policy Content** area, paste a custom policy.

      .. code-block::

         {
             "Version": "1.1",
             "Statement": [
                 {
                     "Effect": "Allow",
                     "Action": [
                         "csms:secretVersion:get",
                         "csms:secretVersion:list",
                         "kms:dek:decrypt"
                     ]
                 }
             ]
         }

   d. Enter a policy description as required.

#. Click **Next**.

#. On the **Select Policy/Role** page, select **Custom policy** from the first drop-down list and select the custom policy created in :ref:`8 <dli_01_0512__en-us_topic_0118645629_li1671455941710>`.

#. Click **Next**. On the **Select Scope** page, set the authorization scope. In this example, select **All resources**.

#. Click **OK**.

   It takes 15 to 30 minutes for the authorization to be in effect.

.. _dli_01_0512__section131564016912:

Step 5: Create a Flink Jar Job and Configure Job Information
------------------------------------------------------------

#. **Create a Flink Jar job.**

   a. In the navigation pane on the left of the DLI management console, choose **Job Management** > **Flink Jobs**.

   b. On the displayed page, click **Create Job** in the upper right corner.

      In this example, set **Type** to **Flink Jar** and **Name** to **Flink_Jar_for_test**.

   c. Click **OK**.

#. **Configure basic job information.**

   Configure basic job information based on :ref:`Table 5 <dli_01_0512__table158872059165310>`.

   .. _dli_01_0512__table158872059165310:

   .. table:: **Table 5** Parameter descriptions

      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter             | Mandatory             | Description                                                                                                                                                                       |
      +=======================+=======================+===================================================================================================================================================================================+
      | Queue                 | Yes                   | Select a queue where you want to run your job.                                                                                                                                    |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Application           | Yes                   | Select the custom package in :ref:`Step 1: Develop a JAR File and Upload It to OBS <dli_01_0512__section31930062913>`.                                                            |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Main Class            | Yes                   | Class name of the JAR file to load                                                                                                                                                |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       | This parameter specifies the entry for the Flink job, that is, the class that contains the **main** method. This is the class that is executed first when a Flink job is started. |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       | If the application program is of type .jar, the main class name must be provided.                                                                                                 |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       | The main class name is case-sensitive and must be correct.                                                                                                                        |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       | -  **Default**: Specified based on the **Manifest** file in the JAR file.                                                                                                         |
      |                       |                       | -  **Manually assign**: You must enter the class name and confirm the class arguments (separated by spaces).                                                                      |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       | .. note::                                                                                                                                                                         |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       |    When a class belongs to a package, the package path must be carried, for example, **packagePath.KafkaMessageStreaming**.                                                       |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Flink Version         | Yes                   | Flink version used for job running                                                                                                                                                |
      |                       |                       |                                                                                                                                                                                   |
      |                       |                       | If you choose to use Flink 1.15, make sure to configure the agency information for the cloud service that DLI is allowed to access in the job.                                    |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Agency                | No                    | If you choose to use Flink 1.15, configure the agency name yourself to ensure smooth job running.                                                                                 |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. **Configure advanced settings for the Flink Jar job.**

   Configure the Flink Jar job based on :ref:`Table 6 <dli_01_0512__table599316568589>`.

   .. _dli_01_0512__table599316568589:

   .. table:: **Table 6** Advanced settings for the Flink Jar job

      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                 | Mandatory             | Description                                                                                                                                                                                                                     |
      +===========================+=======================+=================================================================================================================================================================================================================================+
      | CUs                       | Yes                   | One CU consists of one vCPU and 4 GB of memory. The number of CUs ranges from 2 to 400.                                                                                                                                         |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Job Manager CUs           | Yes                   | Number of CUs allowed for JobManager. The value ranges from 1 to 4. The default value is **1**.                                                                                                                                 |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parallelism               | Yes                   | Maximum number of parallel operators in a job                                                                                                                                                                                   |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | .. note::                                                                                                                                                                                                                       |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       |    -  The value cannot exceed four times the number of compute units (**CUs** - **Job Manager CUs**).                                                                                                                           |
      |                           |                       |    -  Set this parameter to a value greater than that configured in the code. Otherwise, job submission may fail.                                                                                                               |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Task Manager Config       | No                    | Whether TaskManager resource parameters are set                                                                                                                                                                                 |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | If this option is selected, you need to set the following parameters:                                                                                                                                                           |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | -  **CU(s) per TM**: Number of resources occupied by each TaskManager.                                                                                                                                                          |
      |                           |                       | -  **Slot(s) per TM**: Number of slots contained in each TaskManager.                                                                                                                                                           |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Save Job Log              | No                    | Whether job running logs are saved to OBS                                                                                                                                                                                       |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | If this option is selected, you need to set the following parameters:                                                                                                                                                           |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | **OBS Bucket**: Select an OBS bucket to store job logs. If the OBS bucket you selected is unauthorized, click **Authorize**.                                                                                                    |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm on Job Exception    | No                    | Whether to notify users of any job exceptions, such as running exceptions or arrears, via SMS or email.                                                                                                                         |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | If this option is selected, you need to set the following parameters:                                                                                                                                                           |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | **SMN Topic**                                                                                                                                                                                                                   |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | Select a custom SMN topic. For details about how to create a custom SMN topic, see "Creating a Topic" in the *Simple Message Notification User Guide*.                                                                          |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Auto Restart on Exception | No                    | Whether automatic restart is enabled. If enabled, jobs will be automatically restarted and restored when exceptions occur.                                                                                                      |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | If this option is selected, you need to set the following parameters:                                                                                                                                                           |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | -  **Max. Retry Attempts**: maximum number of retries upon an exception. The unit is times/hour.                                                                                                                                |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       |    -  **Unlimited**: The number of retries is unlimited.                                                                                                                                                                        |
      |                           |                       |    -  **Limited**: The number of retries is user-defined.                                                                                                                                                                       |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       | -  **Restore Job from Checkpoint**: Restore the job from the latest checkpoint.                                                                                                                                                 |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       |    If you select this parameter, you also need to set **Checkpoint Path**.                                                                                                                                                      |
      |                           |                       |                                                                                                                                                                                                                                 |
      |                           |                       |    **Checkpoint Path**: Select a path for storing checkpoints. This path must match that configured in the application package. Each job must have a unique checkpoint path, or, you will not be able to obtain the checkpoint. |
      +---------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Save** in the upper right of the page.

#. Click **Start** in the upper right corner.

#. On the displayed **Start Flink Job** page, confirm the job specifications and click **Start Now** to start the job.

   Once the job is started, the system automatically switches to the **Flink Jobs** page. Locate the job you created and check its status in the **Status** column.

   Once a job is successfully submitted, its status changes from **Submitting** to **Running**. After the execution is complete, the status changes to **Completed**.

   If the job status is **Submission failed** or **Running exception**, the job fails to submit or run. In this case, you can hover over the status icon in the **Status** column of the job list to view the error details. You can click |image1| to copy these details. Rectify the fault based on the error information and resubmit the job.

.. |image1| image:: /_static/images/en-us_image_0000001956063772.png
