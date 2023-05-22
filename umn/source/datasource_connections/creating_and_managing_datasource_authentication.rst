:original_name: dli_01_0427.html

.. _dli_01_0427:

Creating and Managing Datasource Authentication
===============================================

Scenarios
---------

Datasource authentication is used to manage authentication information for accessing CSS and MRS security environments and encrypt passwords for accessing DWS, RDS, DDS, and DCS data sources.

-  When creating a CSS cluster in the security mode, you need to specify the username and password. A certificate is available for downloading the security CSS cluster. To access the security CSS cluster, the username, password, and certificate need to be stored in DLI. The username, password, and certificate are the datasource authentication information for accessing the CSS cluster.
-  When creating an MRS cluster in the security mode, you need to enable the kerberos authentication. Download the authentication credential, which contains the **krb5.conf** and **user.keytab** files. When connecting DLI to an MRS cluster, you can store the two files in DLI. The two files are the datasource authentication information for accessing the MRS cluster.
-  To create a Kafka cluster, you need to enable SSL access. You can configure this function on the **Service Configuration** page of MRS and Kafka, or set up an open-source Kafka cluster and modify the configuration file to enable this function.
-  Through datasource password authentication, the passwords for accessing DWS, RDS, DDS, and DCS data sources can be encrypted for storage.

Creating a Datasource Authentication
------------------------------------

#. Create a data source to be accessed.

   .. note::

      If a cluster is available, you do not need to apply for one.

   -  Create a CSS cluster in the security mode: Select 6.5.4 or a later version for **Cluster Version** and enable the **Security Mode**.
   -  Create an MRS cluster in the security mode: On the MRS management console, apply for a cluster, select **Custom Config**, set **Cluster Version** to MRS 2.1.0 or later, and enable **Kerberos Authentication**.
   -  Creating a Kafka cluster: You can configure the Kafka cluster on the Service Configuration page of MRS and Kafka. For details about how to configure SSL on MRS, see **Security Configuration** in the *MapReduce Service User Guide*.
   -  Creating a DWS cluster: Apply for a data warehouse cluster from DWS. For details, see the *Data Warehouse Service Management Guide*.
   -  Creating an RDS data source: Apply for a database instance in the RDS service. For details, see the *Relational Database Service Getting Started*.
   -  Creating a DCS data source: Apply for a DCS instance in DCS. For details, see the *Distributed Cache Service User Guide*.
   -  Creating a DDS data source: Apply for a DDS DB instance. For details, see *Document Database Service Getting Started*.

#. Download the authentication credential. Skip this step when you access DWS, RDS, DCS, or DDS data sources.

   -  CSS security cluster: On the **Cluster Management** page, click the cluster name. On the **Basic Information** page that is displayed, find the **Security Mode** and download the certificate of the CSS security cluster.
   -  MRS security cluster: On the cluster list page, click the cluster name. On the displayed cluster information page, download the authentication credential. After downloading the authentication credential, decompress it to obtain the **krb5.conf** and **user.keytab** files.

#. Upload the authentication credential. Skip this step when you access DWS, RDS, DCS, or DDS data sources.

   Upload the obtained authentication credential file to the user-defined OBS bucket.

#. On the DLI management console, click **Datasource Connections**.

#. On the **Datasource Authentication** tab page, click **Create** to create an authentication information.

   -  CSS

      .. table:: **Table 1** Parameters

         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                        |
         +===================================+====================================================================================================================================================+
         | Type                              | Select CSS.                                                                                                                                        |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication information to be created.                                                                                   |
         |                                   |                                                                                                                                                    |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                 |
         |                                   | -  The length of the database name cannot exceed 128 characters.                                                                                   |
         |                                   | -  It is recommended that the name contain the CSS security cluster name to distinguish security authentication information of different clusters. |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username                          | Username for logging in to the security cluster.                                                                                                   |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Password                          | The password of the security cluster                                                                                                               |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Certificate Path                  | The OBS path for uploading the security certificate                                                                                                |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+

   -  MRS

      .. table:: **Table 2** Description

         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                 |
         +===================================+=============================================================================================================================================================================+
         | Type                              | Select **Kerberos**.                                                                                                                                                        |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication information to be created.                                                                                                            |
         |                                   |                                                                                                                                                                             |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                                          |
         |                                   | -  The length of the database name cannot exceed 128 characters.                                                                                                            |
         |                                   | -  It is recommended that the name contain the MRS security cluster name to distinguish security authentication information of different clusters.                          |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username                          | Username for logging in to the security cluster.                                                                                                                            |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | **krb5_conf** Path                | OBS path to which the **krb5.conf** file is uploaded                                                                                                                        |
         |                                   |                                                                                                                                                                             |
         |                                   | .. note::                                                                                                                                                                   |
         |                                   |                                                                                                                                                                             |
         |                                   |    The **renew_lifetime** configuration item under **[libdefaults]** must be removed from **krb5.conf**. Otherwise, the **Message stream modified (41)** problem may occur. |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | **keytab** Path                   | OBS path to which the **user.keytab** file is uploaded                                                                                                                      |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  Kafka

      .. table:: **Table 3** Parameters

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                        |
         +===================================+====================================================================================================================================+
         | Type                              | Select **Kafka_SSL**.                                                                                                              |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication information to be created.                                                                   |
         |                                   |                                                                                                                                    |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). |
         |                                   | -  The length of the database name cannot exceed 128 characters.                                                                   |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Truststore Path                   | OBS path to which the SSL truststore file is uploaded.                                                                             |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Truststore Password               | Truststore password. The default value is **dms@kafka**.                                                                           |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Keystore Path                     | OBS path to which the SSL keystore file (key and certificate) is uploaded.                                                         |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Keystore Password                 | Keystore (key and certificate) password.                                                                                           |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Key Password                      | Password of the private key in the keystore file.                                                                                  |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

   -  Password (datasource password authentication)

      Create datasource authentication for accessing DWS, RDS, DCS, and DDS data sources.

      .. note::

         Currently, database password authentication supports Spark SQL jobs only.

      .. table:: **Table 4** Parameters

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                        |
         +===================================+====================================================================================================================================+
         | Type                              | Select **Password**.                                                                                                               |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication information to be created.                                                                   |
         |                                   |                                                                                                                                    |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). |
         |                                   | -  The length of the database name cannot exceed 128 characters.                                                                   |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Username                          | Username for accessing the datasource                                                                                              |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Password                          | Password for accessing the datasource                                                                                              |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

Searching for a Datasource Authentication
-----------------------------------------

On the **Datasource Authentication** tab, you can enter the authentication information name in the search box to search for the matching authentication information. To ensure user information security, the password field is not returned.

Updating Authentication Information
-----------------------------------

On the **Datasource Authentication** tab, click **Update** in the **Operation** column of the authentication information to be modified. Currently, only the username and password can be updated. If you need to update the certificate, delete the authentication information and create a new one.

.. note::

   The username and password are optional. If they are not set, the field is not modified.

Deleting a Datasource Authentication
------------------------------------

On the **Datasource Authentication** tab, click **Delete** in the **Operation** column of the authentication information to be deleted.
