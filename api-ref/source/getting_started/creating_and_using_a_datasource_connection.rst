:original_name: dli_02_0311.html

.. _dli_02_0311:

Creating and Using a Datasource Connection
==========================================

Scenario Description
--------------------

This section describes how to create an enhanced datasource connection using an API.

Constraints
-----------

-  It takes 6 to 10 minutes to start a job using a new queue for the first time.
-  Before creating an enhanced datasource connection, you need to obtain the ID of the VPC and the network ID of the subnet where the service is located.

Involved APIs
-------------

-  :ref:`Creating a Queue <dli_02_0194>`: Create a dedicated queue.
-  :ref:`Creating an Enhanced Datasource Connection <dli_02_0187>`: Create an enhanced datasource connection.
-  :ref:`Binding a Queue <dli_02_0191>`: Bind a queue.
-  :ref:`Querying an Enhanced Datasource Connection <dli_02_0189>`: Check whether an enhanced datasource connection is successfully created.

Procedure
---------

#. Create a queue. For details, see :ref:`Creating a Queue <dli_02_0307>`. In the request, set **resource_mode** to **1** to create a dedicated queue.
#. Create an enhanced datasource connection.

   -  API

      URI format: POST /v2.0/{*project_id*}/datasource/enhanced-connections

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the request parameters, see :ref:`Creating an Enhanced Datasource Connection <dli_02_0187>`.

   -  Request example

      -  Description: Create an enhanced datasource connection named **test1** in project **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: POST https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/datasource/enhanced-connections

      -  Body:

         .. code-block::

            {
              "name": "test1",
              "dest_vpc_id": "22094d8f-c310-4621-913d-4c4d655d8495",
              "dest_network_id": "78f2562a-36e4-4b39-95b9-f5aab22e1281",
              "queues": ["q1","q2"],
              "hosts": [
                {
                  "ip":"192.168.0.1",
                  "name":"ecs-97f8-0001"
                },
                {
                  "ip":"192.168.0.2",
                  "name":"ecs-97f8-0002"
                }
              ]
            }

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": "",
           "connection_id": "2a620c33-5609-40c9-affd-2b6453071b0f"
         }

#. (Optional) If no queue is bound when you create an enhanced datasource connection, you can use the :ref:`Binding a Queue <dli_02_0191>` API to bind a queue.
#. Verify that the enhanced datasource connection is created successfully.

   -  API

      URI format: GET /v2.0/{*project_id*}/datasource/enhanced-connections/{*connection_id*}

      -  Obtain the value of {*project_id*} from :ref:`Obtaining a Project ID <dli_02_0183>`.
      -  For details about the query parameters, see :ref:`Creating a Database <dli_02_0028>`.

   -  Request example

      -  Description: Query an enhanced datasource connection whose ID is **2a620c33-5609-40c9-affd-2b6453071b0f** in project **48cc2c48765f481480c7db940d6409d1**.

      -  Example URL: GET https://{*endpoint*}/v2.0/48cc2c48765f481480c7db940d6409d1/datasource/enhanced-connections/2a620c33-5609-40c9-affd-2b6453071b0f

      -  Body:

         .. code-block::

            {}

   -  Example response

      .. code-block::

         {
           "is_success": true,
           "message": "",
           "name": "test1",
           "id": "2a620c33-5609-40c9-affd-2b6453071b0f",
           "available_queue_info": [
             {
               "status": "ACTIVE",
               "name": "queue1",
               "peer_id": "2a620c33-5609-40c9-affd-2b6453071b0f",
               "err_msg": "",
               "update_time": 1566889577861
             }
           ],
           "dest_vpc_id": "22094d8f-c310-4621-913d-4c4d655d8495",
           "dest_network_id": "78f2562a-36e4-4b39-95b9-f5aab22e1281",
           "isPrivis": true,
           "create_time": 1566888011125,
           "status": "ACTIVE",
           "hosts": [
             {
               "ip":"192.168.0.1",
               "name":"ecs-97f8-0001"
             },
             {
               "ip":"192.168.0.2",
               "name":"ecs-97f8-0002"
             }
           ]
         }
