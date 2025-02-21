:original_name: dli_01_0560.html

.. _dli_01_0560:

Creating a Kafka_SSL Datasource Authentication
==============================================

Scenario
--------

Create a Kafka_SSL datasource authentication on the DLI console to store the Kafka authentication information to DLI. This will allow you to access to Kafka instances without having to configure a username and password in SQL jobs.

.. note::

   -  When Kerberos authentication is enabled for MRS Kafka but SSL authentication is disabled, create a Kerberos authentication. When creating a table, configure **krb_auth_name** to associate the datasource authentication.
   -  If Kerberos authentication and SSL authentication are both enabled for MRS Kafka, you need to create Kerberos and Kafka_SSL authentications. When creating a table, configure **krb_auth_name** and **ssl_auth_name** to associate the datasource authentications.
   -  Datasource authentication is not required when Kerberos authentication is disabled but SASL authentication is enabled for MRS Kafka (for example, when a username and a password are used for PlainLoginModule authentication).
   -  When Kerberos authentication is disabled but SSL authentication is enabled for MRS Kafka, you need to create a Kafka_SSL authentication. When creating a table, configure **ssl_auth_name** to associate the datasource authentication.
   -  When Kerberos authentication is disabled but SASL authentication and SSL authentication are enabled for MRS Kafka, you need to create a Kafka_SSL authentication. When creating a table, configure **ssl_auth_name** to associate the datasource authentication.

Procedure
---------

#. Download the authentication credential.

   -  **DMS Kafka**

      a. Log in to the DMS (for Kafka) console and click a Kafka instance to access its details page.

      b. In the connection information, find the SSL certificate and click **Download**.

         Decompress the downloaded **kafka-certs** package to obtain the **client.jks** and **phy_ca.crt** files.

   -  **MRS** **Kafka**

      a. Log in to MRS Manager.
      b. Choose **System** > **Permission** > **User**.
      c. Click **More**, select **Download Authentication Credential**, save the file, and decompress it to obtain the truststore file.

#. Upload the authentication credential to the OBS bucket.

#. Create a datasource authentication.

   a. Log in to the DLI management console.

   b. Choose **Datasource Connections**. On the page displayed, click **Datasource Authentication**.

   c. Click **Create**.

      Configure Kafka authentication parameters according to :ref:`Table 1 <dli_01_0560__table1764213114497>`.

      .. _dli_01_0560__table1764213114497:

      .. table:: **Table 1** Parameters

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                        |
         +===================================+====================================================================================================================================+
         | Type                              | Select **Kafka_SSL**.                                                                                                              |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication to be created.                                                                               |
         |                                   |                                                                                                                                    |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). |
         |                                   | -  The name can contain a maximum of 128 characters.                                                                               |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Truststore Path                   | OBS path to which the SSL truststore file is uploaded.                                                                             |
         |                                   |                                                                                                                                    |
         |                                   | -  For MRS Kafka, enter the OBS path of the **Truststore.jks** file.                                                               |
         |                                   | -  For DMS Kafka, enter the OBS path of the **client.jks** file.                                                                   |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Truststore Password               | Truststore password.                                                                                                               |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Keystore Path                     | OBS path to which the SSL keystore file (key and certificate) is uploaded.                                                         |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Keystore Password                 | Keystore (key and certificate) password.                                                                                           |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Key Password                      | Password of the private key in the keystore file.                                                                                  |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

#. Access Kafka with SASL_SSL authentication enabled.

   When creating a data source, associate the data source with the created datasource authentication to access the data source.

   For details about how to create a table, see *Data Lake Insight Syntax Reference*.
