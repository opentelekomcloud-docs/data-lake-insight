:original_name: dli_02_0336.html

.. _dli_02_0336:

Viewing Scaling History of an Elastic Resource Pool
===================================================

Function
--------

This API is used to view scaling history of an elastic resource pool.

URI
---

GET /v3/{project_id}/elastic-resource-pools/{elastic_resource_pool_name}/scale-records

.. table:: **Table 1** URI parameters

   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory       | Type            | Description                                                                                                                                   |
   +============================+=================+=================+===============================================================================================================================================+
   | project_id                 | Yes             | String          | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | elastic_resource_pool_name | Yes             | String          | Elastic resource pool name                                                                                                                    |
   |                            |                 |                 |                                                                                                                                               |
   |                            |                 |                 | The value can contain 1 to 128 characters.                                                                                                    |
   +----------------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------+

.. table:: **Table 2** query parameters

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================================================+
   | start_time      | No              | Long            | Start time of the historical scaling records you want to query. The time must be 30 days earlier than the current time and earlier than the **end_time**. The value is a UNIX timestamp in milliseconds. |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | -  If **start_time** is left empty, data generated in the recent seven days before **end_time** will be queried. The **end_time** cannot be later than 30 days after the current time.                   |
   |                 |                 |                 | -  If both **start_time** and **end_time** are left empty, data generated in the recent 15 days before the current time will be queried.                                                                 |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | end_time        | No              | Long            | End time of the historical scaling records. The value cannot be earlier than the **start_time** or later than the current time. The value is a UNIX timestamp in milliseconds.                           |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | -  If **end_time** is left empty, data generated since the **start_time** will be queried.                                                                                                               |
   |                 |                 |                 | -  If both **start_time** and **end_time** are left empty, data generated in the recent 15 days before the current time will be queried.                                                                 |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | status          | No              | String          | Scaling status                                                                                                                                                                                           |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | Enumerated values:                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | -  **success**                                                                                                                                                                                           |
   |                 |                 |                 | -  **fail**                                                                                                                                                                                              |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | offset          | No              | Integer         | Offset                                                                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | Minimum value: **0**                                                                                                                                                                                     |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | limit           | No              | Integer         | Number of records displayed on a page                                                                                                                                                                    |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | Minimum value: **0**                                                                                                                                                                                     |
   |                 |                 |                 |                                                                                                                                                                                                          |
   |                 |                 |                 | Maximum value: **100**                                                                                                                                                                                   |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 3** Response body parameters

   +-----------+-----------------+------------------------------------------------------------------------------------------------+
   | Parameter | Type            | Description                                                                                    |
   +===========+=================+================================================================================================+
   | count     | Integer         | Number of elements in the array                                                                |
   +-----------+-----------------+------------------------------------------------------------------------------------------------+
   | items     | Array of arrays | Data returned in the array For details, see :ref:`Table 4 <dli_02_0336__table10219162172215>`. |
   +-----------+-----------------+------------------------------------------------------------------------------------------------+

.. _dli_02_0336__table10219162172215:

.. table:: **Table 4** items parameters

   =========== ======= ===============================================
   Parameter   Type    Description
   =========== ======= ===============================================
   max_cu      Integer Maximum number of CUs
   min_cu      Integer Minimum number of CUs
   current_cu  Integer Scaled number of CUs
   origin_cu   Integer Original number of CUs
   target_cu   Integer Target number of CUs
   record_time Long    Operation completion time
   status      String  Scaling status, which can be success or failure
   fail_reason String  Failure cause
   =========== ======= ===============================================

Example Request
---------------

.. code-block:: text

   GET https://{endpoint}/v3/{project_id}/elastic-resource-pools/{elastic_resource_pool_name}/scale-records?start_time=1650784624000&end_time=1652625304002&status=&limit=20&offset=1

Example Response
----------------

The following is an example for a successful query:

.. code-block::

   {
     "count" : 1,
     "items" : [ {
       "max_cu" : 64,
       "min_cu" : 16,
       "current_cu" : 16,
       "target_cu" : 16,
       "origin_cu" : 16,
       "record_time" : 1650784624000,
       "status" : "fail",
       "fail_reason" : "Internal error, please contact technical support."
     } ]
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
