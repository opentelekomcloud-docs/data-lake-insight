:original_name: dli_02_0328.html

.. _dli_02_0328:

Deleting an Elastic Resource Pool
=================================

Function
--------

This API is used to delete an elastic resource pool.

URI
---

-  URI format

   DELETE /v3/{project_id}/elastic-resource-pools/{elastic_resource_pool_name}

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

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the message may be left blank.                                              |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

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
