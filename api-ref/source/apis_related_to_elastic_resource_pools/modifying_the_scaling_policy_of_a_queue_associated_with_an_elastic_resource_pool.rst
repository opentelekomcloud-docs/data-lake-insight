:original_name: dli_02_0332.html

.. _dli_02_0332:

Modifying the Scaling Policy of a Queue Associated with an Elastic Resource Pool
================================================================================

Function
--------

This API is used to modify the scaling policy of a queue associated with an elastic resource pool.

URI
---

-  URI format

   PUT /v3/{project_id}/elastic-resource-pools/{elastic_resource_pool_name}/queues/{queue_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                  | Mandatory | Type   | Description                                                                                                                                   |
      +============================+===========+========+===============================================================================================================================================+
      | elastic_resource_pool_name | Yes       | String | Elastic resource pool name.                                                                                                                   |
      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | project_id                 | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name                 | Yes       | String | Name of a bound queue.                                                                                                                        |
      +----------------------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameter

   +------------------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Mandatory | Type             | Description                                                                                                                                                                                                                                                                             |
   +========================+===========+==================+=========================================================================================================================================================================================================================================================================================+
   | queue_scaling_policies | Yes       | Array of objects | Scaling policy of a queue in an elastic resource pool. A policy contains the period, priority, and CU range. There must be a default scaling policy (period [00:00, 24:00]) for each queue. For details about the parameters, see :ref:`Table 3 <dli_02_0332__request_priority_infos>`. |
   +------------------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0332__request_priority_infos:

.. table:: **Table 3** queue_scaling_policies

   ================= ========= ======= ================================
   Parameter         Mandatory Type    Description
   ================= ========= ======= ================================
   impact_start_time Yes       String  Time when a policy takes effect.
   impact_stop_time  Yes       String  Time when a policy expires.
   priority          Yes       Integer Priority.
   min_cu            Yes       Integer Minimum number of CUs.
   max_cu            Yes       Integer Maximum number of CUs.
   ================= ========= ======= ================================

Response
--------

.. table:: **Table 4** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the message may be left blank.                                              |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Modify the scaling policy of a queue associated with an elastic resource pool.

.. code-block::

   {
     "queue_scaling_policies" : [ {
       "priority" : 100,
       "impact_start_time" : "10:00",
       "impact_stop_time" : "22:00",
       "min_cu":"64",
       "max_cu":"752"
     }, {
       "priority" : 50,
       "impact_start_time" : "22:00",
       "impact_stop_time" : "10:00",
       "min_cu":"64",
       "max_cu":"752"
     } ]
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
