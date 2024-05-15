:original_name: dli_02_0308.html

.. _dli_02_0308:

Creating and Submitting a SQL Job
=================================

Scenario Description
--------------------

This section describes how to create and query SQL jobs using APIs.

Constraints
-----------

-  It takes 6 to 10 minutes to start a job using a new queue for the first time.

Involved APIs
-------------

-  :ref:`Creating a Queue <dli_02_0194>`: Create a queue.
-  :ref:`Creating a Database (Discarded) <dli_02_0028>`: Create a database.
-  :ref:`Creating a Table (Discarded) <dli_02_0034>`: Create a table.
-  :ref:`Importing Data (Discarded) <dli_02_0019>`: Import the data to be queried.
-  :ref:`Querying Job Details <dli_02_0022>`: Check whether the imported data is correct.
-  :ref:`Submitting a SQL Job (Recommended) <dli_02_0102>`: Submit a query job.

Procedure
---------

#. Create a SQL queue. For details, see :ref:`Creating a Queue <dli_02_0307>`.
#. Create a database.

   -  API

      URI format: POST /v1.0/{project_id}/databases

      -  Obtain the value of {project_id} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Database (Discarded) <dli_02_0028>`.

   -  Request example

      -  Description: Creates a database named **db1** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/databases

      -  Body:

         .. code-block::

            {
                 "database_name": "db1",
                 "description": "this is for test"
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }

#. Create a table.

   -  API

      URI format: POST /v1.0/{*project_id*}/databases/{*database_name*}/tables

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Table (Discarded) <dli_02_0034>`.

   -  Request example

      -  Description: In the project whose ID is **48cc2c48765f481480c7db940d6409d1**, create a table named **tb1** in the **db1** database.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/databases/db1/tables

      -  Body:

         .. code-block::

            {
              "table_name": "tb1",
              "data_location": "OBS",
              "description": "",
              "data_type": "csv",
              "data_path": "obs://obs/path1/test.csv",
              "columns": [
              {
                 "column_name": "column1",
                 "type": "string",
                 "description": "",
                 "is_partition_column": true
              },
              {
                 "column_name": "column2",
                 "type": "string",
                 "description": "",
                 "is_partition_column": false
              }
              ],
              "with_column_header": true,
              "delimiter": ",",
              "quote_char": "\"",
              "escape_char": "\\",
              "date_format": "yyyy-MM-dd",
              "timestamp_format": "yyyy-MM-dd HH:mm:ss"
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }

#. (Optional) If the table to be created does not contain data, use the :ref:`Importing Data (Discarded) <dli_02_0019>` API to import data to the table.
#. (Optional) After data is imported, you can use the :ref:`Querying Job Details <dli_02_0022>` API to check whether the imported data is correct.
#. Submit a query job.

   -  API

      URI format: POST /v1.0/{*project_id*}/jobs/submit-job

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Database (Discarded) <dli_02_0028>`.

   -  Request example

      -  Description: Submit a SQL job in the project whose ID is **48cc2c48765f481480c7db940d6409d1** and query data in the **tb1** table in the database **db1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/jobs/submit-job

      -  Body:

         .. code-block::

            {
                "currentdb": "db1",
                "sql": "select * from tb1 limit 10",
                "queue_name": "queue1"
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": "",
           "job_id":""95fcc908-9f1b-446c-8643-5653891d9fd9",
           "job_type": "QUERY",
           "job_mode": "async"
         }
