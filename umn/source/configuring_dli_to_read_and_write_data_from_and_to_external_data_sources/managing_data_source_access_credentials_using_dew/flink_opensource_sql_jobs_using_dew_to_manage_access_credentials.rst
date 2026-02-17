:original_name: dli_09_0210.html

.. _dli_09_0210:

Flink OpenSource SQL Jobs Using DEW to Manage Access Credentials
================================================================

Scenario
--------

When DLI writes the output data of Flink jobs to MySQL or DWS, you need to set sensitive parameters such as the username and password in the connector. Storing this information in plaintext poses significant security risks. To safeguard user data privacy, you are advised to encrypt these credentials.

Data Encryption Workshop (DEW) and Cloud Secret Management Service (CSMS) offer a secure, reliable, and easy-to-use solution for encrypting and decrypting sensitive data. By hosting database account details (for example, username and password) as managed secrets within CSMS, you can securely reference these credentials in your Flink jobs. This ensures that sensitive information is retrieved through a secure channel during runtime.

Additionally, CSMS offers comprehensive lifecycle management for credentials, enhancing both security and efficiency. It effectively mitigates risks associated with hardcoding sensitive information or storing it in plaintext configurations, thereby preventing unauthorized access and potential business disruptions.

This section walks you through on how to use DEW to manage access credentials for Flink OpenSource SQL jobs.

Notes and Constraints
---------------------

DEW can be used to manage access credentials only in Flink 1.15. When creating a Flink job, select version 1.15 and configure the information of the agency that allows DLI to access DEW for the job.

Prerequisites
-------------

-  A shared secret has been created on the DEW console and the secret value has been stored.

-  An agency has been created and authorized for DLI to access DEW. The agency must have been granted the following permissions:

   -  Permission of the **ShowSecretVersion** interface for querying secret versions and secret values in DEW: **csms:secretVersion:get**.
   -  Permission of the **ListSecretVersions** interface for listing secret versions in DEW: **csms:secretVersion:list**.
   -  Permission to decrypt DEW secrets: **kms:dek:decrypt**

-  On the DLI management console, create an enhanced datasource connection and configure the network connection between DLI and the data source.

Syntax
------

.. code-block::

   create table tableName(
     attr_name attr_type
     (',' attr_name attr_type)*
     (',' WATERMARK FOR rowtime_column_name AS watermark-strategy_expression)
   )
   with (
      ...
     'dew.endpoint'='',
     'dew.csms.secretName'='',
     'dew.csms.decrypt.fields'='',
     'dew.projectId'='',
     'dew.csms.version'=''

   );

Parameter Description
---------------------

.. table:: **Table 1** Parameter descriptions

   +-------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory   | Default Value  | Data Type   | Description                                                                                                                                                                                                                                                                                                     |
   +=========================+=============+================+=============+=================================================================================================================================================================================================================================================================================================================+
   | dew.endpoint            | Yes         | None           | String      | Endpoint of the DEW service to be used.                                                                                                                                                                                                                                                                         |
   +-------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dew.projectId           | No          | Yes            | String      | ID of the project DEW belongs to. The default value is the ID of the project where the Flink job is.                                                                                                                                                                                                            |
   +-------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dew.csms.secretName     | Yes         | None           | String      | Name of the shared secret in DEW's secret management.                                                                                                                                                                                                                                                           |
   |                         |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                         |             |                |             | Configuration example: **'dew.csms.secretName'='**\ *secretInfo*\ **'**                                                                                                                                                                                                                                         |
   +-------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dew.csms.decrypt.fields | Yes         | None           | String      | Specify which fields in the **connector with** attribute need to be decrypted using DEW's CSMS.                                                                                                                                                                                                                 |
   |                         |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                         |             |                |             | Separate the field attributes with commas, for example, **'dew.csms.decrypt.fields'='field1,field2,field3'**                                                                                                                                                                                                    |
   +-------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dew.csms.version        | Yes         | Latest version | String      | Version number (secret version identifier) of the shared secret created in DEW CSMS.                                                                                                                                                                                                                            |
   |                         |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                         |             |                |             | If the latest version number (secret version identifier) is not specified, the system will not be able to directly retrieve the most recent version of the secret, potentially leading to application access failures or the use of outdated secrets, thereby compromising data security and service stability. |
   |                         |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                         |             |                |             | View the version information of the secret on the DEW management console and configure this parameter with the latest version number to ensure that applications can securely and reliably access the necessary data.                                                                                           |
   |                         |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                         |             |                |             | Configuration example: **'dew.csms.version'='v1'**                                                                                                                                                                                                                                                              |
   +-------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

This example demonstrates how to configure Flink OpenSource SQL to manage access credentials using DEW by generating random data through a DataGen table and outputting it to a MySQL result table.

#. Create a shared secret in DEW.

   a. Log in to the DEW management console.
   b. In the navigation pane on the left, choose **Cloud Secret Management Service** > **Secrets**.
   c. Click **Create Secret**. On the displayed page, configure basic secret information.

      -  **Secret Name**: Enter a secret name. In this example, the name is **secretInfo**.
      -  **Secret Value**: Enter the username and password for logging in to the RDS for MySQL DB instance.

         -  The key in the first line is **MySQLUsername**, and the value is the username for logging in to the DB instance.
         -  The key in the second line is **MySQLPassword**, and the value is the password for logging in to the DB instance.

   d. Set other parameters as required and click **OK**.

#. Configure DEW to manage access credentials in the Flink OpenSource SQL job.

   Ensure that an enhanced datasource connection between DLI and MySQL has been created.

   Ensure that an agency has been created for DLI to access DEW and authorization has been completed.

   Here is an example configuration for a Flink OpenSource SQL job:

   ::

      create table dataGenSource(
        user_id string,
        amount int
      ) with (
        'connector' = 'datagen',
        'rows-per-second' = '1', --Generate a piece of data per second.
        'fields.user_id.kind' = 'random', --Specify a random generator for the user_id field.
        'fields.user_id.length' = '3' --Limit the length of user_id to 3.
      );

      CREATE TABLE jdbcSink (
        user_id string,
        amount int
       )
      WITH (
       'connector' = 'jdbc',
        'url? = 'jdbc:mysql://MySQLAddress:MySQLPort/flink',--flink is the MySQL database where the orders table locates.
       'table-name' = 'orders',
       'username' = 'MySQLUsername',  -- Shared secret in DEW whose name is secretInfo and version is v1. The key MySQLUsername defines the secret value. The value is the user's sensitive information.
       'password' = 'MySQLPassword',  -- Shared secret in DEW whose name is secretInfo and version is v1. The key MySQLPassword defines the secret value. The value is the user's sensitive information.
       'sink.buffer-flush.max-rows' = '1',
       'dew.endpoint'='endpoint', --Endpoint information for the DEW service being used
       'dew.csms.secretName'='secretInfo', --Name of the DEW shared secret
       'dew.csms.decrypt.fields'='username,password', --The username and password field values must be decrypted and replaced using DEW secret management.
       'dew.csms.version'='v1'
      );

      insert into jdbcSink select * from dataGenSOurce;
