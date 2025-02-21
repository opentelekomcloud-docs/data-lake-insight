:original_name: dli_01_0636.html

.. _dli_01_0636:

Using DEW to Manage Access Credentials for Data Sources
=======================================================

When using DLI to submit jobs that involve reading and writing data from external sources, it is crucial to securely access these sources by properly storing their access credentials. This ensures the authentication of the data source and enables secure access by DLI. DEW is a comprehensive cloud-based encryption service that addresses data security, key security, and complex key management issues. This section describes how to use DEW to store authentication information for a data source.

Creating a Shared Secret in DEW
-------------------------------

This example describes how to configure a credential for accessing RDS DB instances in a DLI job and store the credential in DEW.

#. Log in to the DEW management console.
#. In the navigation pane on the left, choose **Cloud Secret Management Service** > **Secrets**.
#. Click **Create Secret**. On the displayed page, configure basic secret information.

   -  **Secret Name**: Enter a secret name. In this example, the name is **secretInfo**.
   -  **Secret Value**: Enter the username and password for logging in to the RDS for MySQL DB instance.

      -  The key in the first line is **MySQLUsername**, and the value is the username for logging in to the DB instance.
      -  The key in the second line is **MySQLPassword**, and the value is the password for logging in to the DB instance.

#. Set other parameters as required and click **OK**.

Using the Secret Created in DEW in a DLI Job
--------------------------------------------

This part uses a Flink job as an example to describe how to use credentials created in DEW.

.. code-block::

   WITH (
    'connector' = 'jdbc',
     'url? = 'jdbc:mysql://MySQLAddress:MySQLPort/flink',--flink is the MySQL database where the orders table locates.
    'table-name' = 'orders',
    'username' = 'MySQLUsername',  -- Shared secret in DEW whose name is secretInfo and version is v1. The key MySQLUsername defines the secret value. The value is the user's sensitive information.
    'password' = 'MySQLPassword',  -- Shared secret in DEW whose name is secretInfo and version is v1. The key MySQLPassword defines the secret value. The value is the user's sensitive information.
    'sink.buffer-flush.max-rows' = '1',
    'dew.endpoint'='kms.xxxx.com', --Endpoint information for the DEW service being used
    'dew.csms.secretName'='secretInfo', --Name of the DEW shared secret
    'dew.csms.decrypt.fields'='username,password', --The password field value must be decrypted and replaced using DEW secret management.
    'dew.csms.version'='v1'
   );
