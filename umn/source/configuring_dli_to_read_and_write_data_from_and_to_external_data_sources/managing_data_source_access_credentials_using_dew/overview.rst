:original_name: dli_01_0687.html

.. _dli_01_0687:

Overview
========

When submitting Flink or Spark jobs through DLI to access external data sources (such as OBS and Kafka), there is a risk of plaintext exposure if AK/SK, usernames/passwords are directly embedded in the job code or parameter configurations.

To securely store data source access credentials, ensure data source authentication safety, and facilitate secure access to data sources by DLI, you are advised to use DEW for managing data source access credentials. DLI employs "agency + temporary credentials" to safely retrieve data source access credentials.

DEW is a comprehensive cloud-based encryption service designed to address challenges related to data security, key security, and the complexities of key management.

This section describes how to use DEW to store data source authentication information across various job types.

Notes and Constraints
---------------------

You are advised to use DEW for storing data source authentication information exclusively when Spark 3.3.1 or later and Flink 1.15 or later jobs access data sources using datasoure connections.

When SQL and Flink 1.12 jobs access data sources using datasource connections, use DLI's datasource authentication feature to manage data source access credentials. For details, see :ref:`Using DLI Datasource Authentication to Manage Access Credentials for Data Sources <dli_01_0422>`.
