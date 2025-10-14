:original_name: dli_09_0198.html

.. _dli_09_0198:

Troubleshooting
===============

Problem 1
---------

-  Symptom

   The Spark job fails to be executed, and the job log indicates that the Java server connection or container fails to be started.

-  Solution

   Check whether the host information of the datasource connection has been modified. If not, modify the host information by referring to :ref:`Configuring MRS Host Information in DLI Datasource Connection <dli_09_0196__section18700849172311>`. Then create and submit a Spark job again.

Problem 2
---------

-  Symptom

   A Spark job fails to be executed and "KrbException: Message stream modified (41)" is displayed in the job log.

-  Solution

   Delete all **renew_lifetime = xxx** configurations from the **krb5.conf** configuration file. Then create and submit a Spark job again.
