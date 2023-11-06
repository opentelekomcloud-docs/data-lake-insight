:original_name: dli_01_0561.html

.. _dli_01_0561:

Introduction
============

What Is Datasource Authentication?
----------------------------------

Datasource authentication is used to manage authentication information for accessing specified data sources. After datasource authentication is configured, you do not need to repeatedly configure data source authentication information in jobs, improving data source authentication security while enabling DLI to securely access data sources.

Constraints
-----------

-  Compared with datasource authentication provided by DLI, you are advised to use Data Encryption Worksop (DEW) to store data source authentication information.
-  Only Spark SQL and Flink OpenSource SQL 1.12 jobs support datasource authentication.
-  DLI supports four types of datasource authentication. Select an authentication type specific to each data source.

   -  CSS: applies to 6.5.4 or later CSS clusters with the security mode enabled.
   -  Kerberos: applies to MRS security clusters with Kerberos authentication enabled.
   -  Kafka_SSL: applies to Kafka with SSL enabled.
   -  Password: applies to GaussDB(DWS), RDS, DDS, and DCS.

Datasource Authentication Types
-------------------------------

DLI supports four types of datasource authentication. Select an authentication type specific to each data source.

-  CSS: applies to 6.5.4 or later CSS clusters with the security mode enabled. During the configuration, you need to specify the username, password, and authentication certificate of the cluster and store the information in DLI through datasource authentication so that DLI can securely access CSS data sources. For details, see :ref:`Creating a CSS Datasource Authentication <dli_01_0427>`.
-  Kerberos: applies to MRS security clusters with Kerberos authentication enabled. During the configuration, you need to specify MRS cluster authentication credentials, including the **krb5.conf** and **user.keytab** files. For details, see :ref:`Creating a Kerberos Datasource Authentication <dli_01_0558>`.
-  Kafka_SSL: applies to Kafka with SSL enabled. During the configuration, you need to specify the KafkaTruststore path and password. For details, see :ref:`Creating a Kafka_SSL Datasource Authentication <dli_01_0560>`.
-  Password: applies to GaussDB(DWS), RDS, DDS, and DCS data sources. During the configuration, you need to store the passwords of the data sources in DLI. For details, see :ref:`Creating a Password Datasource Authentication <dli_01_0559>`.

Jobs That Can Connect to Data Sources Through Datasource Authentication
-----------------------------------------------------------------------

Different types of jobs can connect to data sources through different types of datasource authentication.

-  For details about the data sources that Spark SQL jobs can connect to through datasource authentication and their constraints, see :ref:`Table 1 <dli_01_0561__table629065545911>`.
-  For details about the data sources that Flink SQL jobs can connect to through datasource authentication and their constraints, see :ref:`Table 2 <dli_01_0561__table208001745193719>`.

.. _dli_01_0561__table629065545911:

.. table:: **Table 1** Data sources that Spark SQL jobs can connect to through datasource authentication

   +--------------------------------+-----------------------------------+---------------------------------------------------------+
   | Datasource Authentication Type | Data Source                       | Constraints                                             |
   +================================+===================================+=========================================================+
   | CSS                            | CSS                               | The CSS cluster version must be 6.5.4 or later.         |
   |                                |                                   |                                                         |
   |                                |                                   | The security mode has been enabled for the CSS cluster. |
   +--------------------------------+-----------------------------------+---------------------------------------------------------+
   | Password                       | GaussDB(DWS), RDS, DDS, and Redis | ``-``                                                   |
   +--------------------------------+-----------------------------------+---------------------------------------------------------+

.. _dli_01_0561__table208001745193719:

.. table:: **Table 2** Data sources that Flink SQL jobs can connect to through datasource authentication

   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   | Table Type      | Datasource Authentication Type | Data Source                | Constraints                                                   |
   +=================+================================+============================+===============================================================+
   | Source table    | Kerberos                       | Kafka                      | Kerberos authentication has been enabled for MRS Kafka.       |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   |                 | Kafka_SSL                      | Kafka                      | SASL_SSL authentication has been enabled for DMS Kafka.       |
   |                 |                                |                            |                                                               |
   |                 |                                |                            | SASL authentication has been enabled for MRS Kafka.           |
   |                 |                                |                            |                                                               |
   |                 |                                |                            | SSL authentication has been enabled for MRS Kafka.            |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   | Result table    | Kerberos                       | HBase                      | Kerberos authentication has been enabled for the MRS cluster. |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   |                 |                                | Kafka                      | Kerberos authentication has been enabled for MRS Kafka.       |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   |                 | Kafka_SSL                      | Kafka                      | SASL_SSL authentication has been enabled for DMS Kafka.       |
   |                 |                                |                            |                                                               |
   |                 |                                |                            | SASL authentication has been enabled for MRS Kafka.           |
   |                 |                                |                            |                                                               |
   |                 |                                |                            | SSL authentication has been enabled for MRS Kafka.            |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   |                 | Password                       | GaussDB(DWS), RDS, and CSS | ``-``                                                         |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
   | Dimension table | Password                       | RDS and Redis              | ``-``                                                         |
   +-----------------+--------------------------------+----------------------------+---------------------------------------------------------------+
