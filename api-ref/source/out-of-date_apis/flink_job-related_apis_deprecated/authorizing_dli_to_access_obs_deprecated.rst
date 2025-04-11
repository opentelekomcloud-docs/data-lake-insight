:original_name: dli_02_0225.html

.. _dli_02_0225:

Authorizing DLI to Access OBS (Deprecated)
==========================================

Function
--------

This API is used to grant DLI the permission to access OBS buckets for saving job checkpoints and run logs.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   POST /v1.0/{project_id}/dli/obs-authorize

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

   =========== ========= ================ ====================
   Parameter   Mandatory Type             Description
   =========== ========= ================ ====================
   obs_buckets Yes       Array of Strings List of OBS buckets.
   =========== ========= ================ ====================

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | Message content.                                                                                                  |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Grant DLI the permission to access the OBS bucket **bucket1** so that DLI can save job checkpoints and run logs to the bucket.

.. code-block::

   {
       "obs_buckets": [
           "bucket1"
       ]
   }

Example Response
----------------

.. code-block::

   {
       "is_success": "true",
       "message": "The following OBS bucket is authorized successfully, bucket1."
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0225__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0225__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Authorization succeeds.
   400         Request error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
