:original_name: dli_02_0329.html

.. _dli_02_0329:

Modifying Elastic Resource Pool Information
===========================================

Function
--------

This API is used to modify elastic resource pool information.

URI
---

-  URI format

   PUT /v3/{project_id}/elastic-resource-pools/{elastic_resource_pool_name}

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

.. table:: **Table 2** Request parameters

   +-------------+-----------+---------+-----------------------------------------------------------------+
   | Parameter   | Mandatory | Type    | Description                                                     |
   +=============+===========+=========+=================================================================+
   | description | No        | String  | Description. The value can contain a maximum of 256 characters. |
   +-------------+-----------+---------+-----------------------------------------------------------------+
   | max_cu      | No        | Integer | Maximum CUs allowed for an elastic resource pool.               |
   +-------------+-----------+---------+-----------------------------------------------------------------+
   | min_cu      | No        | Integer | Maximum CUs allowed for an elastic resource pool.               |
   +-------------+-----------+---------+-----------------------------------------------------------------+

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

Modify the description, maximum CUs, and minimum CUs of the elastic resource pool. After the modification, the minimum CUs is **78** and the maximum CUs is **990**.

.. code-block::

   {
     "description" : "test_update",
     "min_cu" : 78,
     "max_cu" : 990
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
