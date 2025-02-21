:original_name: dli_02_0243.html

.. _dli_02_0243:

Deleting Jobs in Batches
========================

Function
--------

This API is used to batch delete jobs at any state.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/jobs/delete

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameter

   ========= ========= ====== ===========
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========
   job_ids   Yes       [Long] Job ID.
   ========= ========= ====== ===========

Response
--------

.. table:: **Table 3** Response parameters

   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type             | Description                                                                                   |
   +================+===========+==================+===============================================================================================+
   | Array elements | No        | Array of Objects | Returned response message. For details, see :ref:`Table 4 <dli_02_0243__table3383103182713>`. |
   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------+

.. _dli_02_0243__table3383103182713:

.. table:: **Table 4** Array element parameters

   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                       |
   +============+===========+========+===================================================================================================================+
   | is_success | No        | String | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String | Message content.                                                                                                  |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Delete the jobs whose IDs are **12** and **232**.

.. code-block::

   {
     "job_ids":[12,232]
   }

Example Response
----------------

.. code-block::

   [{
       "is_success": "true",
       "message": "The job is deleted successfully.",
   }]

Status Codes
------------

:ref:`Table 5 <dli_02_0243__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0243__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 5** Status codes

   =========== ================================
   Status Code Description
   =========== ================================
   200         The job is deleted successfully.
   400         The input parameter is invalid.
   =========== ================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
