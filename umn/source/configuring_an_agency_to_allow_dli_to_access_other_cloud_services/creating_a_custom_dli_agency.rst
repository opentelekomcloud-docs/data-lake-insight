:original_name: dli_01_0616.html

.. _dli_01_0616:

Creating a Custom DLI Agency
============================

When Flink 1.15, Spark 3.3, or a later version is used to execute jobs and the required agency is not included in the DLI system agency **dli_management_agency**, you need to create an agency on the IAM console and add information about the new agency to the job configuration. **dli_management_agency** contains the permissions required for datasource operations, message notifications, and user authorization operations. For other agency permission requirements, you need to create custom DLI agencies. For details about **dli_management_agency**, see :ref:`DLI Agency Overview <dli_01_0419>`.

This section walks you through on how to create a custom agency, complete service authorization, and add information about the new agency to the job configuration.

DLI Custom Agency Scenarios
---------------------------

.. table:: **Table 1** DLI custom agency scenarios

   +----------------------------------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Scenario                                                             | Agency Name           | Description                                                                                                                                                                                                                                                                                               | Permission Policy                                                                        |
   +======================================================================+=======================+===========================================================================================================================================================================================================================================================================================================+==========================================================================================+
   | Allowing DLI to clear data according to the lifecycle of a table     | dli_data_clean_agency | Data cleanup agency, which is used to clean up data according to the lifecycle of a table and clean up lakehouse table data.                                                                                                                                                                              | :ref:`Data Cleanup Agency Permission Configuration <dli_01_0617__section1320218173917>`  |
   |                                                                      |                       |                                                                                                                                                                                                                                                                                                           |                                                                                          |
   |                                                                      |                       | You need to create an agency and customize permissions for it. However, the agency name is fixed to **dli_data_clean_agency**.                                                                                                                                                                            |                                                                                          |
   +----------------------------------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Allowing DLI to read and write data from and to OBS to transfer logs | Custom                | For DLI Flink jobs, the permissions include downloading OBS objects, obtaining OBS/GaussDB(DWS) data sources (foreign tables), transferring logs, using savepoints, and enabling checkpointing. For DLI Spark jobs, the permissions allow downloading OBS objects and reading/writing OBS foreign tables. | :ref:`Permission Policies for Accessing and Using OBS <dli_01_0617__section02775191915>` |
   +----------------------------------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Allowing DLI to obtain data access credentials by accessing DEW      | Custom                | DLI jobs use DEW-CSMS' secret management.                                                                                                                                                                                                                                                                 | :ref:`Permission to Use DEW's Encryption Function <dli_01_0617__section1943510461382>`   |
   +----------------------------------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Allowing DLI to access DLI catalogs to retrieve metadata             | Custom                | DLI accesses catalogs to retrieve metadata.                                                                                                                                                                                                                                                               | :ref:`Permission to Access DLI Catalog Metadata <dli_01_0617__section1563832810618>`     |
   +----------------------------------------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+

Procedure
---------


.. figure:: /_static/images/en-us_image_0000001842062193.png
   :alt: **Figure 1** Process of creating a custom agency

   **Figure 1** Process of creating a custom agency

Notes and Constraints
---------------------

-  The custom agency name cannot be the default agency names **dli_admin_agency**, **dli_management_agency**, or **dli_data_clean_agency**. It must be unique.
-  The name of the agency that allows DLI to clear data according to the lifecycle of a table must be **dli_data_clean_agency**.
-  Custom agencies can be configured only for jobs executed by engines like Flink 1.15, Spark 3.3.1 (Spark general queue scenario), or later versions.
-  Once the agency permissions are updated, your **dli_admin_agency** will be upgraded to **dli_management_agency**. This new agency will have the necessary permissions for datasource operations, notifications, and user authorization operations. If you have other agency permission needs, you will need to create custom agencies. For details about **dli_management_agency**, see :ref:`DLI Agency Overview <dli_01_0419>`.
-  Common scenarios for creating an agency: DLI is allowed to read and write data from and to OBS, dump logs, and read and write Flink checkpoints. DLI is allowed to access DEW to obtain data access credentials and access catalogs to obtain metadata. For details about agency permissions in these scenarios, see :ref:`Agency Permission Policies in Common Scenarios <dli_01_0617>`.

Step 1: Create a Cloud Service Agency on the IAM Console and Grant Permissions
------------------------------------------------------------------------------

#. Log in to the management console.

#. In the upper right corner of the page, hover over the username and select **Identity and Access Management**.

#. In the navigation pane of the IAM console, choose **Agencies**.

#. On the displayed page, click **Create Agency**.

#. On the **Create Agency** page, set the following parameters:

   -  **Agency Name**: Enter an agency name, for example, **dli_obs_agency_access**.
   -  **Agency Type**: Select **Cloud service**.
   -  **Cloud Service**: This parameter is available only when you select **Cloud service** for **Agency Type**. Select **Data Lake Insight (DLI)** from the drop-down list.
   -  **Validity Period**: Select **Unlimited**.
   -  **Description**: You can enter **Agency with OBS OperateAccess permissions**. This parameter is optional.

#. Click **Next**.

#. Click the agency name. On the displayed page, click the **Permissions** tab. Click **Authorize**. On the displayed page, click **Create Policy**.

#. .. _dli_01_0616__en-us_topic_0118645629_li1671455941710:

   Configure policy information.

   a. Enter a policy name, for example, **dli-obs-agency**.

   b. Select **JSON**.

   c. In the **Policy Content** area, paste a custom policy.

      In this example, the permissions allow access and usage of OBS in various scenarios. For DLI Flink jobs, this includes downloading OBS objects, obtaining OBS/GaussDB(DWS) data sources (foreign tables), transferring logs, using savepoints, and enabling checkpointing. For DLI Spark jobs, the permissions allow downloading OBS objects and reading/writing OBS foreign tables.

      For how to configure common agency permissions for Flink jobs, see :ref:`Agency Permission Policies in Common Scenarios <dli_01_0617>`.

      .. code-block::

         {
             "Version": "1.1",
             "Statement": [
                 {
                     "Effect": "Allow",
                     "Action": [
                         "obs:bucket:GetBucketPolicy",
                         "obs:bucket:GetLifecycleConfiguration",
                         "obs:bucket:GetBucketLocation",
                         "obs:bucket:ListBucketMultipartUploads",
                         "obs:bucket:GetBucketLogging",
                         "obs:object:GetObjectVersion",
                         "obs:bucket:GetBucketStorage",
                         "obs:bucket:GetBucketVersioning",
                         "obs:object:GetObject",
                         "obs:object:GetObjectVersionAcl",
                         "obs:object:DeleteObject",
                         "obs:object:ListMultipartUploadParts",
                         "obs:bucket:HeadBucket",
                         "obs:bucket:GetBucketAcl",
                         "obs:bucket:GetBucketStoragePolicy",
                         "obs:object:AbortMultipartUpload",
                         "obs:object:DeleteObjectVersion",
                         "obs:object:GetObjectAcl",
                         "obs:bucket:ListBucketVersions",
                         "obs:bucket:ListBucket",
                         "obs:object:PutObject"
                     ],
                     "Resource": [
                         "OBS:*:*:bucket:bucketName",// Replace bucketName with the actual bucket name.
                         "OBS:*:*:object:*"
                     ]
                 },
                 {
                     "Effect": "Allow",
                     "Action": [
                         "obs:bucket:ListAllMyBuckets"
                     ]
                 }
             ]
         }

   d. Enter a policy description as required.

#. Click **Next**.

#. On the **Select Policy/Role** page, select **Custom policy** from the first drop-down list and select the custom policy created in :ref:`8 <dli_01_0616__en-us_topic_0118645629_li1671455941710>`.

#. Click **Next**. On the **Select Scope** page, set the authorization scope.

   In this example, the custom policy is an OBS agency. So, select **Global services**. If a DLI agency is used, you are advised to select **Region-specific projects**.

#. Click **OK**.

   It takes 15 to 30 minutes for the authorization to be in effect.

Step 2: Set Agency Permissions for a Job
----------------------------------------

When Flink 1.15, Spark 3.3, or a later version is used to execute jobs, you need to add information about the new agency to the job configuration.

Otherwise, If you do not specify an agency for Spark 3.3.1 jobs, the jobs cannot use OBS. If you do not specify an agency for a Flink 1.15 job, checkpointing cannot be enabled, savepoints cannot be used, logs cannot be transferred, and data sources such as OBS and GaussDB(DWS) cannot be used.

.. caution::

   -  You can only specify an agency for Flink 1.15 and Spark 3.3.1 jobs running on queues in an elastic resource pool.
   -  After specifying an agency for a job, be careful when modifying the permissions granted to the agency. Any changes made may impact the job's normal operation.

-  **Specifying an agency for a Flink Jar job**

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. Select a desired job and click **Edit** in the **Operation** column.
   #. In the job configuration area on the right, configure agency information.

      -  **Flink Version**: Select **1.15**.

      -  **Runtime Configuration**: Configure the key-value information of the new agency. The key is fixed to **flink.dli.job.agency.name**, and the value is the custom agency name.

         In this example, set the key-value information to **flink.dli.job.agency.name=dli_obs_agency_access**.

-  **Specifying an agency for a Flink OpenSource SQL job**

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. Select a desired job and click **Edit** in the **Operation** column.
   #. In the job configuration area on the right, configure agency information.

      -  On the **Running Parameters** tab, ensure that the selected Flink version is **1.15**.

      -  Click **Runtime Configuration**. Configure the key-value information of the new agency. The key is fixed to **flink.dli.job.agency.name**, and the value is the custom agency name.

         In this example, set the key-value information to **flink.dli.job.agency.name=dli_obs_agency_access**.

-  **Specifying an agency for a Spark job**

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Spark Jobs**.
   #. Select the target job and click **Edit** in the **Operation** column.
   #. In the job configuration area on the right, configure agency information.

      -  Ensure that the selected Spark version is **3.3.1**.

      -  In the **Spark Arguments(--conf)** area, configure the key-value information of the new agency. The key is fixed to **spark.dli.job.agency.name**, and the value is the custom agency name.

         In this example, set the key-value information to **spark.dli.job.agency.name=dli_obs_agency_access**.
