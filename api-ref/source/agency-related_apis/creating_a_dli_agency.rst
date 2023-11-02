:original_name: dli_02_0299.html

.. _dli_02_0299:

Creating a DLI Agency
=====================

Function
--------

This API is used to create an agency for a DLI user.

URI
---

-  URI format

   POST /v2/{project_id}/agency

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                            |
   +=================+=================+==================+========================================================================================================================+
   | roles           | Yes             | Array of Strings | Role.                                                                                                                  |
   |                 |                 |                  |                                                                                                                        |
   |                 |                 |                  | Currently, only **obs_adm**, **dis_adm**, **ctable_adm**, **vpc_netadm**, **smn_adm**, and **te_admin** are supported. |
   |                 |                 |                  |                                                                                                                        |
   |                 |                 |                  | -  **obs_adm**: Administrator permissions for accessing and using the Object Storage Service.                          |
   |                 |                 |                  | -  **dis_adm**: Administrator permissions for using Data Ingestion Service data as the data source                     |
   |                 |                 |                  | -  **ctable_adm**: Administrator permissions for accessing and using the CloudTable service                            |
   |                 |                 |                  | -  **vpc_netadm**: Administrator permissions for using the Virtual Private Cloud service                               |
   |                 |                 |                  | -  **smn_adm**: Administrator permissions for using the Simple Message Notification service                            |
   |                 |                 |                  | -  **te_admin**: Tenant Administrator permissions                                                                      |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Create a DLI user agency. The agency has the following permissions: **CloudTable Administrator** for accessing and using CloudTable, **VPC Administrator** for using VPC, **DIS Administrator** for accessing and using DIS, **SMN Administrator** for using SMN, and accessing and using OBS.

.. code-block::

   {
       "roles": [
           "ctable_adm",
           "vpc_netadm",
           "dis_adm",
           "smn_adm",
           "obs_adm"
       ]
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

:ref:`Table 4 <dli_02_0299__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0299__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ================================
   Status Code Description
   =========== ================================
   200         The job is created successfully.
   400         Request failure.
   =========== ================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
