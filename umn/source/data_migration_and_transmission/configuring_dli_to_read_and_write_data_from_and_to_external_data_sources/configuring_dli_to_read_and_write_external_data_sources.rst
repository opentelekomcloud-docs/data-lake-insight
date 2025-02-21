:original_name: dli_01_0635.html

.. _dli_01_0635:

Configuring DLI to Read and Write External Data Sources
=======================================================

To read and write external data sources when running DLI jobs, two conditions must be met:

-  Establish network connectivity between DLI and the external data source to ensure that the DLI queue is connected to the data source network.
-  Securely store the access credentials for the data source to ensure authentication security and facilitate secure DLI access to the data source.

This section describes how to configure DLI to read and write external data sources.

-  Configure network connection between DLI and the data source by referring to :ref:`Configuring the Network Connection Between DLI and Data Sources (Enhanced Datasource Connection) <dli_01_0426>`.
-  Manage credentials for DLI to access data sources.

   -  Spark 3.3.1 or later and Flink 1.15 or later jobs accessing data sources using datasource connections

      -  You are advised to use Data Encryption Workshop (DEW) to store authentication information of data sources, addressing data security, key security, and complex key management issues.

         For details, see :ref:`Using DEW to Manage Access Credentials for Data Sources <dli_01_0636>`.

      -  To manage data source access credentials using DEW, you also need to create a DLI agency to grant DLI access to read access credentials for other services (DEW).

   -  When SQL and Flink 1.12 jobs access data sources using datasource connections, use DLI's datasource authentication feature to manage data source access credentials. For details, see :ref:`Using DLI Datasource Authentication to Manage Access Credentials for Data Sources <dli_01_0422>`.
