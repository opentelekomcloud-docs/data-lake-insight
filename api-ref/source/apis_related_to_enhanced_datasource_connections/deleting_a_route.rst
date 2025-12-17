:original_name: dli_02_0372.html

.. _dli_02_0372:

Deleting a Route
================

Function
--------

This API is used to delete a datasource connection route.

URI
---

-  URI format

   DELETE /v3/{project_id}/datasource/enhanced-connections/{connection_id}/routes/{name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | connection_id | Yes       | String | Datasource connection ID                                                                                                                      |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | name          | Yes       | String | Route name                                                                                                                                    |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response parameters

   +------------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Type    | Description                                                                                                       |
   +============+=========+===================================================================================================================+
   | is_success | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | String  | System prompt. If the execution succeeds, the message may be left blank.                                          |
   +------------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "Deleted"
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
