:original_name: dli_09_0215.html

.. _dli_09_0215:

Spark Jar Jobs Using DEW to Acquire Access Credentials for Reading and Writing Data from and to OBS
===================================================================================================

What Is a Temporary Credential?
-------------------------------

A temporary security credential grants temporary access rights. It includes a temporary AK/SK and a security token, both of which must be used together.

Scenario
--------

When writing output data from Spark Jar jobs to OBS, you need to configure an AK/SK for accessing OBS. To ensure the security of AK/SK data, you can use DEW and CSMS for centralized management of AK/SK. This approach effectively mitigates risks such as sensitive information leakage caused by hardcoding in programs or plaintext configurations, as well as potential business disruptions due to unauthorized access.

This section walks you through on how a Spark Jar job acquires an AK/SK to read and write data from and to OBS.

Notes and Constraints
---------------------

-  DEW can be used to manage access credentials only in Spark 3.3.1 (Spark general queue scenario) or later. When creating a Spark job, select version 3.3.1 and configure the information of the agency that allows DLI to access DEW for the job.

-  To use this function, you need to configure AK/SK for all OBS buckets.

Prerequisites
-------------

-  A shared secret has been created on the DEW console and the secret value has been stored.
-  An agency has been created and authorized for DLI to access DEW. The agency must have been granted the following permissions:

   -  Permission of the **ShowSecretVersion** interface for querying secret versions and secret values in DEW: **csms:secretVersion:get**.
   -  Permission of the **ListSecretVersions** interface for listing secret versions in DEW: **csms:secretVersion:list**.
   -  Permission to decrypt DEW secrets: **kms:dek:decrypt**

Syntax
------

On the Spark Jar job editing page, configure the **Spark Arguments(--conf)** parameter as needed. The configuration information is as follows:

Different OBS buckets use different AK/SK authentication information. You can use the following configuration method to specify the AK/SK information based on the bucket. For details about the parameters, see :ref:`Table 1 <dli_09_0215__en-us_topic_0000001883313257_en-us_topic_0000001841630985_table517231215112>`.

::

   spark.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key= USER_AK_CSMS_KEY
   spark.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key= USER_SK_CSMS_KEY
   spark.hadoop.fs.obs.security.provider = com.dli.provider.UserObsBasicCredentialProvider
   spark.hadoop.fs.dew.csms.secretName= CredentialName
   spark.hadoop.fs.dew.endpoint=ENDPOINT
   spark.hadoop.fs.dew.csms.version=VERSION_ID
   spark.hadoop.fs.dew.csms.cache.time.second =CACHE_TIME
   spark.dli.job.agency.name=USER_AGENCY_NAME

Parameter Description
---------------------

.. _dli_09_0215__en-us_topic_0000001883313257_en-us_topic_0000001841630985_table517231215112:

.. table:: **Table 1** Parameter descriptions

   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                                  | Mandatory   | Default Value  | Data Type   | Description                                                                                                                                                                                                                                                                                                     |
   +============================================================+=============+================+=============+=================================================================================================================================================================================================================================================================================================================+
   | spark.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key | Yes         | None           | String      | *USER_BUCKET_NAME* needs to be replaced with the user's OBS bucket name.                                                                                                                                                                                                                                        |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The value of this parameter is the key defined by the user in the CSMS shared secret. The value corresponding to the key is the user's access key ID (AK). The user must have the permission to access the bucket on OBS.                                                                                       |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key | Yes         | None           | String      | *USER_BUCKET_NAME* needs to be replaced with the user's OBS bucket name.                                                                                                                                                                                                                                        |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The value of this parameter is the key defined by the user in the CSMS shared secret. The value corresponding to the key is the user's secret access key (SK). The user must have the permission to access the bucket on OBS.                                                                                   |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.obs.security.provider                      | Yes         | None           | String      | OBS AK/SK authentication mechanism, which uses DEW-CSMS' secret management to obtain the AK and SK for accessing OBS.                                                                                                                                                                                           |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The default value is **com.dli.provider.UserObsBasicCredentialProvider**.                                                                                                                                                                                                                                       |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.dew.csms.secretName                        | Yes         | None           | String      | Name of the shared secret in DEW's secret management.                                                                                                                                                                                                                                                           |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | Configuration example: **spark.hadoop.fs.dew.csms.secretName=secretInfo**                                                                                                                                                                                                                                       |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.dew.endpoint                               | Yes         | None           | String      | Endpoint of the DEW service to be used.                                                                                                                                                                                                                                                                         |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.dew.csms.version                           | Yes         | Latest version | String      | Version number (secret version identifier) of the shared secret created in DEW CSMS.                                                                                                                                                                                                                            |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | If the latest version number (secret version identifier) is not specified, the system will not be able to directly retrieve the most recent version of the secret, potentially leading to application access failures or the use of outdated secrets, thereby compromising data security and service stability. |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | View the version information of the secret on the DEW management console and configure this parameter with the latest version number to ensure that applications can securely and reliably access the necessary data.                                                                                           |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | Configuration example: **spark.hadoop.fs.dew.csms.version=v1**                                                                                                                                                                                                                                                  |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.dew.csms.cache.time.second                 | No          | 3600           | Long        | Cache duration after the CSMS shared secret is obtained during Spark job access.                                                                                                                                                                                                                                |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The unit is second. The default value is 3600 seconds.                                                                                                                                                                                                                                                          |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.hadoop.fs.dew.projectId                              | No          | Yes            | String      | ID of the project DEW belongs to. The default value is the ID of the project where the Spark job is.                                                                                                                                                                                                            |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spark.dli.job.agency.name                                  | Yes         | ``-``          | String      | Custom agency name.                                                                                                                                                                                                                                                                                             |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Sample Code
-----------

This section describes how to write processed DataGen data to OBS. You need to modify the parameters in the sample Java code based on site requirements.

#. Create an agency for DLI to access DEW and complete authorization.

#. Create a shared secret in DEW.

   a. Log in to the DEW management console.
   b. In the navigation pane on the left, choose **Cloud Secret Management Service** > **Secrets**.
   c. On the displayed page, click **Create Secret**. Set basic secret information.

#. Set job parameters on the DLI Spark Jar job editing page.

   Spark Arguments

   ::

      spark.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key= USER_AK_CSMS_KEY
      spark.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key= USER_SK_CSMS_KEY
      spark.hadoop.fs.obs.security.provider=com.dli.provider.UserObsBasicCredentialProvider
      spark.hadoop.fs.dew.csms.secretName=obsAkSk
      spark.hadoop.fs.dew.endpoint=kmsendpoint
      spark.hadoop.fs.dew.csms.version=v3
      spark.dli.job.agency.name=agency
