:original_name: dli_02_0308.html

.. _dli_02_0308:

Creating and Submitting a SQL Job
=================================

Scenario
--------

This section describes how to submit a SQL job to create a database and table and query data using APIs.

Involved APIs
-------------

-  :ref:`Creating an Elastic Resource Pool <dli_02_0326>`
-  :ref:`Creating a Queue <dli_02_0194>`
-  :ref:`Submitting a SQL Job (Recommended) <dli_02_0102>`

Procedure
---------

#. Create an elastic resource pool named **elastic_pool_dli**.

   -  API

      URI format: POST /v3/{project_id}/elastic-resource-pools

      -  Obtain the value of *{project_id}* by referring to :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating an Elastic Resource Pool <dli_02_0326>`.

   -  Example request

      -  Description: Create an elastic resource pool named **elastic_pool_dli** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{endpoint}/v3/48cc2c48765f481480c7db940d6409d1/elastic-resource-pools

      -  Body:

         .. code-block::

            {
              "elastic_resource_pool_name" : "elastic_pool_dli",
              "description" : "test",
              "cidr_in_vpc" : "172.16.0.0/14",
              "charging_mode" : "1",
              "max_cu" : 64,
              "min_cu" : 64
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }

#. Create a queue named **queue1** in the elastic resource pool.

   -  API

      URI format: POST /v1.0/{project_id}/queues

      -  Obtain the value of *{project_id}* by referring to :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating a Queue <dli_02_0194>`.

   -  Example request

      -  Description: Create an elastic resource pool named **queue1** in the project whose ID is **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/queues

      -  Body:

         .. code-block::

            {
                "queue_name": "queue1",
                "queue_type": "sql",
                "description": "test",
                "cu_count": 16,
                "enterprise_project_id": "elastic_pool_dli"
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }

#. Submit a SQL job: Create the database **db1**, table **tb1**, insert data into the table, and query the data.

   -  API

      URI format: POST /v1.0/{*project_id*}/jobs/submit-job

      -  Obtain the value of *{project_id}* by referring to :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Submitting a SQL Job (Recommended) <dli_02_0102>`.

   -  Example request

      -  Description: In the project whose ID is **48cc2c48765f481480c7db940d6409d1**, submit a SQL job, create the database **db1** and table **tb1**, insert data into the table, and query the data.

      -  Example URL: POST https://{*endpoint*}/v1.0/48cc2c48765f481480c7db940d6409d1/jobs/submit-job

      -  Create a database named **db1**.

         .. code-block::

            {
                "queue_name": "queue1",
                "sql": "create DATABASE db1"
            }

      -  Create a table named **tb1**.

         .. code-block::

            {
                "currentdb": "db1",
                "queue_name": "queue1",
                "sql": "create table\n  my_table (id int, name string)"
            }

      -  Insert data into the **tb1** table.

         .. code-block::

            {
                "currentdb": "db1",
                "queue_name": "queue1",
                "sql": "insert into tb1 (id, name) values (1, 'Ann'), (2, 'Jane')"
            }

      -  Query data in the table.

         .. code-block::

            {
                "currentdb": "db1",
                "queue_name": "queue1",
                "sql": "select * from tb1 limit 10",
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": ""
         }
