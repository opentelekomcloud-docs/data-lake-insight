:original_name: dli_01_0417.html

.. _dli_01_0417:

DLI Resources
=============

A resource is an object that exists within a service. You can select DLI resources by specifying their paths.

.. table:: **Table 1** DLI resources and their paths

   +----------------+---------------------------------------------+------------------------------------------------+
   | Resource Type  | Resource Names                              | Path                                           |
   +================+=============================================+================================================+
   | queue          | DLI queue                                   | queues.queuename                               |
   +----------------+---------------------------------------------+------------------------------------------------+
   | database       | DLI database                                | databases.dbname                               |
   +----------------+---------------------------------------------+------------------------------------------------+
   | table          | DLI table                                   | databases.dbname.tables.tbname                 |
   +----------------+---------------------------------------------+------------------------------------------------+
   | column         | DLI column                                  | databases.dbname.tables.tbname.columns.colname |
   +----------------+---------------------------------------------+------------------------------------------------+
   | jobs           | DLI Flink job                               | jobs.flink.jobid                               |
   +----------------+---------------------------------------------+------------------------------------------------+
   | resource       | DLI package                                 | resources.resourcename                         |
   +----------------+---------------------------------------------+------------------------------------------------+
   | group          | DLI package group                           | groups.groupname                               |
   +----------------+---------------------------------------------+------------------------------------------------+
   | datasourceauth | DLI cross-source authentication information | datasourceauth.name                            |
   +----------------+---------------------------------------------+------------------------------------------------+
   | edsconnections | Enhanced datasource connection              | edsconnections.\ *connection ID*               |
   +----------------+---------------------------------------------+------------------------------------------------+
