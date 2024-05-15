:original_name: dli_09_0150.html

.. _dli_09_0150:

Flink Jar Job Examples
======================

Overview
--------

You can perform secondary development based on Flink APIs to build your own Jar packages and submit them to the DLI queues to interact with data sources such as MRS Kafka, HBase, Hive, HDFS, GaussDB(DWS), and DCS.

This section describes how to interact with MRS through a custom job.

Environment Preparations
------------------------

#. Log in to the MRS management console and create an MRS cluster. During the creation, enable **Kerberos Authentication** and select **Kafka**, **HBase**, and **HDFS**. For details about how to create an MRS cluster, see "Buying a Custom Cluster" in .

#. Enable the UDP/TCP port in the security group rule. For details, see "Adding a Security Group Rule" in .

#. Log in to **MRS Manager**.

   a. Create a machine-machine account. Ensure that you have the **hdfs_admin** and **hbase_admin** permissions. Download the user authentication credentials, including the **user.keytab** and **krb5.conf** files.

      .. note::

         The **.keytab** file of a human-machine account becomes invalid when the user password expires. Use a machine-machine account for configuration.

   b. Click **Services**, download the client, and click **OK**.
   c. Download the configuration files from the MRS node, including **hbase-site.xml** and **hiveclient.properties**.

#. Create a dedicated DLI queue.

#. Set up an enhanced datasource connection between the DLI dedicated queue and the MRS cluster and configure security group rules based on the site requirements.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see "Security Group" in *Virtual Private Cloud User Guide*.

#. Obtain the IP address and domain name mapping of all nodes in the MRS cluster, and configure the host mapping in the host information of the DLI cross-source connection.

   For details about how to add an IP-domain mapping, see **Modifying the Host Information** in the *Data Lake Insight User Guide*.

   .. note::

      If the Kafka server listens on the port using **hostname**, you need to add the mapping between the hostname and IP address of the Kafka Broker node to the DLI queue. Contact the Kafka service deployment personnel to obtain the hostname and IP address of the Kafka Broker node.

Prerequisites
-------------

-  Ensure that a dedicated queue has been created.
-  When running a Flink Jar job, you need to build the secondary development application code into a JAR package and upload it to the created OBS bucket. On the DLI console, choose **Data Management** > **Package Management** to create a package.

   .. note::

      DLI does not support the download function. If you need to modify the uploaded data file, edit the local file and upload it again.

-  Flink dependencies have been built in the DLI server and security hardening has been performed based on the open-source community version. **To prevent dependency compatibility issues or log output and dump issues, exclude the following files during packaging**:

   -  Built-in dependencies (or **set the package dependency scope to "provided" in Maven or sbt**)
   -  Log configuration files (for example, **log4j.properties** or **logback.xml**)
   -  JAR packages for log output implementation (for example, **log4j**)

How to Use
----------

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. In the upper right corner of the **Flink Jobs** page, click **Create Job**.

#. Configure job parameters.

   .. table:: **Table 1** Job parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                         |
      +===================================+=====================================================================================================================+
      | Type                              | Select **Flink Jar**.                                                                                               |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------+
      | Name                              | Job name, which contains 1 to 57 characters and consists of only letters, digits, hyphens (-), and underscores (_). |
      |                                   |                                                                                                                     |
      |                                   | .. note::                                                                                                           |
      |                                   |                                                                                                                     |
      |                                   |    The job name must be globally unique.                                                                            |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------+
      | Description                       | Description of the job, which contains 0 to 512 characters.                                                         |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------+

#. Click **OK** to enter the **Edit** page.

#. Select a queue. A Flink Jar job can run only on general queues.

   .. note::

      -  A Flink Jar job can run only on a pre-created dedicated queue.
      -  If no dedicated queue is available in the **Queue** drop-down list, create a dedicated queue and bind it to the current user.

#. Upload the JAR package.

   The Flink version must be the same as that specified in the JAR package.

   .. table:: **Table 2** Description

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                    |
      +===================================+================================================================================================================================================================================================================================================+
      | Application                       | User-defined package. Before selecting a package, upload the corresponding JAR package to the OBS bucket and create a package on the **Data Management** > **Package Management** page.                                                        |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Main Class                        | Name of the main class of the JAR package to be loaded, for example, **KafkaMessageStreaming**.                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                |
      |                                   | -  **Default**: The value is specified based on the **Manifest** file in the JAR package.                                                                                                                                                      |
      |                                   | -  **Manually assign**: You must enter the class name and confirm the class arguments (separate arguments with spaces).                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                |
      |                                   | .. note::                                                                                                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                |
      |                                   |    When a class belongs to a package, the package path must be carried, for example, **packagePath.KafkaMessageStreaming**.                                                                                                                    |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Class Arguments                   | List of arguments of a specified class. The arguments are separated by spaces.                                                                                                                                                                 |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | JAR Package Dependencies          | User-defined dependencies. Before selecting a package, upload the corresponding JAR package to the OBS bucket and create a JAR package on the **Data Management** > **Package Management** page.                                               |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Other Dependencies                | User-defined dependency files. Before selecting a file, upload the corresponding file to the OBS bucket and create a package of any type on the **Data Management** > **Package Management** page.                                             |
      |                                   |                                                                                                                                                                                                                                                |
      |                                   | You can add the following content to the application to access the corresponding dependency file: **fileName** indicates the name of the file to be accessed, and **ClassName** indicates the name of the class that needs to access the file. |
      |                                   |                                                                                                                                                                                                                                                |
      |                                   | .. code-block::                                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                |
      |                                   |    ClassName.class.getClassLoader().getResource("userData/fileName")                                                                                                                                                                           |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Flink Version                     | Before selecting a Flink version, you need to select the queue to which the Flink version belongs. Currently, the following versions are supported: 1.10.                                                                                      |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Configure job parameters.

   .. table:: **Table 3** Parameter description

      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                           | Description                                                                                                                                                                                                                                               |
      +=====================================+===========================================================================================================================================================================================================================================================+
      | CUs                                 | One CU has one vCPU and 4 GB memory. The number of CUs ranges from 2 to 400.                                                                                                                                                                              |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Job Manager CUs                     | Set the number of CUs on a management unit. The value ranges from 1 to 4. The default value is **1**.                                                                                                                                                     |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parallelism                         | Maximum number of parallel operators in a job.                                                                                                                                                                                                            |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | .. note::                                                                                                                                                                                                                                                 |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     |    -  The value must be less than or equal to four times the number of compute units (CUs minus the number of job manager CUs).                                                                                                                           |
      |                                     |    -  You are advised to set this parameter to a value greater than that configured in the code. Otherwise, job submission may fail.                                                                                                                      |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Task Manager Configuration          | Whether to set Task Manager resource parameters.                                                                                                                                                                                                          |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                     |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | -  **CU(s) per TM**: Number of resources occupied by each Task Manager.                                                                                                                                                                                   |
      |                                     | -  **Slot(s) per TM**: Number of slots contained in each Task Manager.                                                                                                                                                                                    |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Save Job Log                        | Whether to save the job running logs to OBS.                                                                                                                                                                                                              |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                     |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | **OBS Bucket**: Select an OBS bucket to store user job logs. If the selected OBS bucket is not authorized, click **Authorize**.                                                                                                                           |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm Generation upon Job Exception | Whether to report job exceptions, for example, abnormal job running or exceptions due to an insufficient balance, to users via SMS or email.                                                                                                              |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                     |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | **SMN Topic**                                                                                                                                                                                                                                             |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | Select a user-defined SMN topic. For details about how to customize SMN topics, see "Creating a Topic" in the *Simple Message Notification User Guide*.                                                                                                   |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Auto Restart upon Exception         | Whether to enable automatic restart. If this function is enabled, jobs will be automatically restarted and restored when exceptions occur.                                                                                                                |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                     |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | -  **Max. Retry Attempts**: maximum number of retry times upon an exception. The unit is **Times/hour**.                                                                                                                                                  |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     |    -  **Unlimited**: The number of retries is unlimited.                                                                                                                                                                                                  |
      |                                     |    -  **Limited**: The number of retries is user-defined.                                                                                                                                                                                                 |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     | -  **Restore Job from Checkpoint**: Restore the job from the latest checkpoint.                                                                                                                                                                           |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     |    If you select this parameter, you also need to set **Checkpoint Path**.                                                                                                                                                                                |
      |                                     |                                                                                                                                                                                                                                                           |
      |                                     |    **Checkpoint Path**: Select the checkpoint saving path. The value must be the same as the checkpoint path you set in the application package. Note that the checkpoint path for each job must be unique. Otherwise, the checkpoint cannot be obtained. |
      +-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Save** on the upper right of the page.

#. Click **Start** on the upper right side of the page. On the displayed **Start Flink Job** page, confirm the job specifications, and click **Start Now** to start the job.

   After the job is started, the system automatically switches to the **Flink Jobs** page, and the created job is displayed in the job list. You can view the job status in the **Status** column. After a job is successfully submitted, the job status will change from **Submitting** to **Running**. After the execution is complete, the message **Completed** is displayed.

   If the job status is **Submission failed** or **Running exception**, the job submission failed or the job did not execute successfully. In this case, you can move the cursor over the status icon in the **Status** column of the job list to view the error details. You can click |image1| to copy these details. After handling the fault based on the provided information, resubmit the job.

   .. note::

      Other buttons are as follows:

      **Save As**: Save the created job as a new job.

Related Operations
------------------

-  **How Do I Configure Job Parameters?**

   #. In the Flink job list, select your desired job.

   #. Click **Edit** in the **Operation** column.

   #. Configure the parameters as needed.

      List of parameters of a specified class. The parameters are separated by spaces.

      Parameter input format: --Key 1 Value 1 --Key 2 Value 2

      For example, if you enter the following parameters on the console:

      --bootstrap.server 192.168.168.xxx:9092

      The parameters are parsed by ParameterTool as follows:


      .. figure:: /_static/images/en-us_image_0000001618446021.png
         :alt: **Figure 1** Parsed parameters

         **Figure 1** Parsed parameters

-  **How Do I View Job Logs?**

   #. In the Flink job list, click a job name to access its details page.

   #. Click the **Run Log** tab and view job logs on the console.

      Only the latest run logs are displayed. For more information, see the OBS bucket that stores logs.

.. |image1| image:: /_static/images/en-us_image_0000001102485176.png
