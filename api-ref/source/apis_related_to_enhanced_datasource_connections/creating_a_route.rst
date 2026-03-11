:original_name: dli_02_0371.html

.. _dli_02_0371:

Creating a Route
================

Function
--------

This API is used to create a datasource connection route.

URI
---

-  URI format

   POST /v3/{project_id}/datasource/enhanced-connections/{connection_id}/routes

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | connection_id | Yes       | String | Datasource connection ID                                                                                                                      |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request parameters

   +-----------+-----------+--------+--------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                            |
   +===========+===========+========+========================================================+
   | name      | Yes       | String | Route name. The value can contain up to 64 characters. |
   +-----------+-----------+--------+--------------------------------------------------------+
   | cidr      | Yes       | String | Route network range                                    |
   +-----------+-----------+--------+--------------------------------------------------------+

Response Parameters
-------------------

.. table:: **Table 3** Response parameters

   +------------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Type    | Description                                                                                                       |
   +============+=========+===================================================================================================================+
   | is_success | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | String  | System prompt. If the execution succeeds, the message may be left blank.                                          |
   +------------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Create a route. Set the next-hop address of the enhanced datasource connection to **127.0.0.0**.

.. code-block::

   {
     "name": "route",
     "cidr": "127.0.0.0"
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": ""
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

For details, see :ref:`Error Codes <dli_02_0056>`.
