:original_name: dli_02_0331.html

.. _dli_02_0331:

Associating a Queue with an Elastic Resource Pool
=================================================

Function
--------

This API is used to associate a queue with an elastic resource pool.

URI
---

-  URI format

   POST /v3/{project_id}/elastic-resource-pools/{elastic_resource_pool_name}/queues

-  Parameter description

   .. table:: **Table 1** URI parameters

      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                  | Mandatory | Type   | Description                                                                                                                                   |
      +============================+===========+========+===============================================================================================================================================+
      | elastic_resource_pool_name | Yes       | String | Elastic resource pool name.                                                                                                                   |
      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | project_id                 | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameter

   ========== ========= ====== ===========
   Parameter  Mandatory Type   Description
   ========== ========= ====== ===========
   queue_name Yes       String Queue name.
   ========== ========= ====== ===========

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the message may be left blank.                                              |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Associate the **lhm_sql** queue with the elastic resource pool.

.. code-block::

   {
     "queue_name" : "lhm_sql"
   }

Example Response
----------------

.. code-block::

   {
     "is_success" : true,
     "message" : ""
   }

Status Codes
------------

=========== ===========
Status Code Description
=========== ===========
200         OK
=========== ===========

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
