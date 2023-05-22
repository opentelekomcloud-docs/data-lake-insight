:original_name: dli_02_0172.html

.. _dli_02_0172:

Querying Resource Packages in a Group
=====================================

Function
--------

This API is used to query resource information of a package group in a **Project**.

URI
---

-  URI format

   GET /v2.0/{project_id}/resources/{resource_name}

-  Parameter description

   .. table:: **Table 1** URI parameter description

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | resource_name | Yes       | String | Name of the resource package that is uploaded.                                                                                                |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** **query** parameter description

      +-----------+-----------+--------+---------------------------------------------------------------------------+
      | Parameter | Mandatory | Type   | Description                                                               |
      +===========+===========+========+===========================================================================+
      | group     | No        | String | Name of the package group returned when the resource package is uploaded. |
      +-----------+-----------+--------+---------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      GET /v2.0/{project_id}/resources/{resource_name}?group=\ *{group}*

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                                           |
   +=======================+=======================+=======================================================================================================+
   | create_time           | Long                  | UNIX time when a resource package is uploaded. The timestamp is expressed in milliseconds.            |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | update_time           | Long                  | UNIX time when the uploaded resource package is uploaded. The timestamp is expressed in milliseconds. |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | resource_type         | String                | Resource type.                                                                                        |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | resource_name         | String                | Resource name.                                                                                        |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | status                | String                | -  Value **UPLOADING** indicates that the resource package group is being uploaded.                   |
   |                       |                       | -  Value **READY** indicates that the resource package has been uploaded.                             |
   |                       |                       | -  Value **FAILED** indicates that the resource package fails to be uploaded.                         |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | underlying_name       | String                | Name of the resource packages in a queue.                                                             |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+
   | owner                 | String                | Owner of a resource package.                                                                          |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "create_time": 1522055409139,
       "update_time": 1522228350501,
       "resource_type": "jar",
       "resource_name": "luxor-ommanager-dist.tar.gz",
       "status": "uploading",
       "underlying_name": "7885d26e-c532-40f3-a755-c82c442f19b8_luxor-ommanager-dist.tar.gz"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0172__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0172__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   ============ ========================
   Status Codes Description
   ============ ========================
   200          The query is successful.
   400          Request error.
   500          Internal service error.
   ============ ========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
