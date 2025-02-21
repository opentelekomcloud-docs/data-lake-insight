:original_name: dli_01_0558.html

.. _dli_01_0558:

Creating a Kerberos Datasource Authentication
=============================================

Scenario
--------

Create a Kerberos datasource authentication on the DLI console to store the authentication information of the data source to DLI. This will allow you to access to the data source without having to configure a username and password in SQL jobs.

.. note::

   -  When Kerberos authentication is enabled for MRS Kafka but SSL authentication is disabled, create a Kerberos authentication. When creating a table, configure **krb_auth_name** to associate the datasource authentication.
   -  If Kerberos authentication and SSL authentication are both enabled for MRS Kafka, you need to create Kerberos and Kafka_SSL authentications. When creating a table, configure **krb_auth_name** and **ssl_auth_name** to associate the datasource authentications.
   -  Datasource authentication is not required when Kerberos authentication is disabled but SASL authentication is enabled for MRS Kafka (for example, when a username and a password are used for PlainLoginModule authentication).
   -  When Kerberos authentication is disabled but SSL authentication is enabled for MRS Kafka, you need to create a Kafka_SSL authentication. When creating a table, configure **ssl_auth_name** to associate the datasource authentication.
   -  When Kerberos authentication is disabled but SASL authentication and SSL authentication are enabled for MRS Kafka, you need to create a Kafka_SSL authentication. When creating a table, configure **ssl_auth_name** to associate the datasource authentication.

Procedure
---------

#. Download the authentication credential of the data source.

   a. Log in to MRS Manager.
   b. Choose **System** > **Permission** > **User**.
   c. Click **More**, select **Download Authentication Credential**, save the file, and decompress it to obtain the **keytab** and **krb5.conf** files.

#. Upload the authentication credential to the OBS bucket.

#. Create a datasource authentication.

   a. Log in to the DLI management console.

   b. Choose **Datasource Connections**. On the page displayed, click **Datasource Authentication**.

   c. Click **Create**.

      Configure Kerberos authentication parameters according to :ref:`Table 1 <dli_01_0558__table172727243398>`.

      .. _dli_01_0558__table172727243398:

      .. table:: **Table 1** Parameters

         +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                             |
         +===================================+=========================================================================================================================================================================+
         | Type                              | Select **Kerberos**.                                                                                                                                                    |
         +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication to be created.                                                                                                                    |
         |                                   |                                                                                                                                                                         |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                                      |
         |                                   | -  The name can contain a maximum of 128 characters.                                                                                                                    |
         |                                   | -  It is recommended that the name contain the MRS security cluster name to distinguish security authentication information of different clusters.                      |
         +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username                          | Username for logging in to the security cluster.                                                                                                                        |
         +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | krb5_conf Path                    | OBS path to which the **krb5.conf** file is uploaded.                                                                                                                   |
         |                                   |                                                                                                                                                                         |
         |                                   | .. note::                                                                                                                                                               |
         |                                   |                                                                                                                                                                         |
         |                                   |    The **renew_lifetime** configuration item under **[libdefaults]** must be removed from **krb5.conf**. Otherwise, the "Message stream modified (41)" error may occur. |
         +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | keytab Path                       | OBS path to which the **user.keytab** file is uploaded.                                                                                                                 |
         +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Create a table to access the MRS cluster.

   When creating a data source, associate the data source with the created datasource authentication to access the data source.

   For details about how to create a table, see *Data Lake Insight Syntax Reference*.
