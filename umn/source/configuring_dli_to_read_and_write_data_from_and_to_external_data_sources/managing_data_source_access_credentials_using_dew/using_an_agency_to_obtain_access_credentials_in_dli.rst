:original_name: dli_01_0687.html

.. _dli_01_0687:

Using an Agency to Obtain Access Credentials in DLI
===================================================

When you use DLI to perform big data processing or cross-service data queries, authentication is a critical prerequisite to ensure legitimate access and data security. Embedding AK/SK or usernames and passwords directly in job code or configuration files introduces the risk of plaintext credential leakage.

To address diverse access scenarios and authentication protocol requirements, DLI provides the following solutions:

**DLI Agency with DEW Temporary Credentials and Permanent AK/SK**

This solution is suitable for services that support only fine-grained authorization (version = 1.1) or workloads that require highly stable credentials without frequent rotation. It provides reliable authentication for these scenarios.

In this approach, DLI uses agencies and temporary credentials to access the DEW service. DEW then provides permanent AK/SK, which are used to securely access other cloud services.

Related guidance:

-  :ref:`Flink OpenSource SQL Jobs Using DEW to Manage Access Credentials <dli_09_0210>`
-  :ref:`Flink Jar Jobs Using DEW to Acquire Access Credentials for Reading and Writing Data from and to OBS <dli_09_0211>`
-  :ref:`Spark Jar Jobs Using DEW to Acquire Access Credentials for Reading and Writing Data from and to OBS <dli_09_0215>`

Use Cases
---------

-  Address security risks caused by hard-coded credentials.
-  Avoid embedding sensitive information, such as data source usernames and passwords, in job code.
-  Enable dynamic credential acquisition and periodic credential rotation.

Notes and Constraints
---------------------

You are advised to use DEW for storing data source authentication information exclusively when Spark 3.3.1 or later and Flink 1.15 or later jobs access data sources using datasource connections.

When SQL and Flink 1.12 jobs access data sources using datasource connections, use DLI's datasource authentication feature to manage data source access credentials. For details, see :ref:`Overview <dli_01_0561>`.

Learn More: What Are Temporary Security Credentials?
----------------------------------------------------

A temporary security credential grants temporary access rights. It includes a temporary AK/SK and a security token, both of which must be used together.
