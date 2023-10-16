:original_name: dli_02_0298.html

.. _dli_02_0298:

Obtaining DLI Agency Information
================================

Function
--------

This API is used to obtain the agency information of a DLI user.

URI
---

-  URI format

   GET /v2/{project_id}/agency

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +-----------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                 |
   +=================+=================+==================+=============================================================================================================================+
   | is_success      | No              | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-----------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message         | No              | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +-----------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | version         | No              | String           | Agency version information.                                                                                                 |
   +-----------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | current_roles   | No              | Array of Strings | Role. The supported values are as follows:                                                                                  |
   |                 |                 |                  |                                                                                                                             |
   |                 |                 |                  | **obs_adm**: Administrator permissions for accessing and using the Object Storage Service.                                  |
   |                 |                 |                  |                                                                                                                             |
   |                 |                 |                  | **dis_adm**: Administrator permissions for using Data Ingestion Service data as the data source                             |
   |                 |                 |                  |                                                                                                                             |
   |                 |                 |                  | **ctable_adm**: Administrator permissions for accessing and using the CloudTable service                                    |
   |                 |                 |                  |                                                                                                                             |
   |                 |                 |                  | **vpc_netadm**: Administrator permissions for using the Virtual Private Cloud service                                       |
   |                 |                 |                  |                                                                                                                             |
   |                 |                 |                  | **smn_adm**: Administrator permissions for using the Simple Message Notification service                                    |
   |                 |                 |                  |                                                                                                                             |
   |                 |                 |                  | **te_admin**: Tenant Administrator permissions                                                                              |
   +-----------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "",
       "version": "v2",
       "current_roles": [
           "ctable_adm",
           "vpc_netadm",
           "ief_adm",
           "dis_adm",
           "smn_adm",
           "obs_adm"
       ]
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0298__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0298__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 3** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         The agency information is obtained.
   400         Request failure.
   404         Not found.
   500         Internal service error.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 4** Error codes

   ========== ==========================
   Error Code Error Message
   ========== ==========================
   DLI.0002   The object does not exist.
   DLI.0999   An internal error occurre
   ========== ==========================
