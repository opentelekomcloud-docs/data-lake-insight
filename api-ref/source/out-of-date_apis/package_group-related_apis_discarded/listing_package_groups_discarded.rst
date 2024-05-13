:original_name: dli_02_0168.html

.. _dli_02_0168:

Listing Package Groups (Discarded)
==================================

Function
--------

This API is used to query all resources in a project, including groups.

.. note::

   This API has been discarded and is not recommended.

URI
---

-  URI format

   GET /v2.0/{project_id}/resources

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------------+-----------------+-----------------+------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                          |
      +=================+=================+=================+======================================================+
      | kind            | No              | String          | Specifies the file type. The options are as follows: |
      |                 |                 |                 |                                                      |
      |                 |                 |                 | -  **jar**: JAR file                                 |
      |                 |                 |                 | -  **pyFile**: User Python file                      |
      |                 |                 |                 | -  **file**: User file                               |
      |                 |                 |                 | -  **modelFile**: User AI model file                 |
      +-----------------+-----------------+-----------------+------------------------------------------------------+
      | tags            | No              | String          | Specifies a label for filtering.                     |
      +-----------------+-----------------+-----------------+------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-----------+-----------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type             | Description                                                                                                                                                               |
   +===========+===========+==================+===========================================================================================================================================================================+
   | resources | No        | Array of objects | List of names of uploaded user resources. For details about resources, see :ref:`Table 4 <dli_02_0168__en-us_topic_0142813184_en-us_topic_0103345070_table111231336220>`. |
   +-----------+-----------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | modules   | No        | Array of objects | List of built-in resource groups. For details about the groups, see :ref:`Table 5 <dli_02_0168__en-us_topic_0142813184_table788814512135>`.                               |
   +-----------+-----------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | groups    | No        | Array of objects | Uploaded package groups of a user.                                                                                                                                        |
   +-----------+-----------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total     | Yes       | Integer          | Total number of returned resource packages.                                                                                                                               |
   +-----------+-----------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0168__en-us_topic_0142813184_en-us_topic_0103345070_table111231336220:

.. table:: **Table 4** resources parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                   |
   +=================+=================+=================+===============================================================================+
   | create_time     | No              | Long            | UNIX timestamp when a resource package is uploaded.                           |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | update_time     | No              | Long            | UNIX timestamp when the uploaded resource package is uploaded.                |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | resource_type   | No              | String          | Resource type.                                                                |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | resource_name   | No              | String          | Resource name.                                                                |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | status          | No              | String          | -  Value **UPLOADING** indicates that the resource package is being uploaded. |
   |                 |                 |                 | -  Value **READY** indicates that the resource package has been uploaded.     |
   |                 |                 |                 | -  Value **FAILED** indicates that the resource package fails to be uploaded. |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | underlying_name | No              | String          | Name of the resource package in the queue.                                    |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | owner           | No              | String          | Owner of the resource package.                                                |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

.. _dli_02_0168__en-us_topic_0142813184_table788814512135:

.. table:: **Table 5** modules parameters

   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                |
   +=================+=================+==================+============================================================================+
   | module_name     | No              | String           | Module name.                                                               |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | module_type     | No              | String           | Module type.                                                               |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | status          | No              | String           | -  Value **UPLOADING** indicates that the package group is being uploaded. |
   |                 |                 |                  | -  Value **READY** indicates that the package group has been uploaded.     |
   |                 |                 |                  | -  Value **FAILED** indicates that the package group fails to be uploaded. |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | resources       | No              | Array of Strings | List of names of resource packages contained in the group.                 |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | description     | No              | String           | Module description.                                                        |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | create_time     | No              | Long             | UNIX timestamp when a package group is uploaded.                           |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+
   | update_time     | No              | Long             | UNIX timestamp when a package group is updated.                            |
   +-----------------+-----------------+------------------+----------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "resources": [
           {
               "create_time": 1521532893736,
               "update_time": 1521552364503,
               "resource_type": "jar",
               "resource_name": "luxor-router-1.1.1.jar",
               "status": "READY",
               "underlying_name": "3efffb4f-40e9-455e-8b5a-a23b4d355e46_luxor-router-1.1.1.jar"
           }
       ],
       "groups": [
           {
               "group_name": "groupTest",
               "status": "READY",
               "resources": [
                   "part-00000-9dfc17b1-2feb-45c5-b81d-bff533d6ed13.csv.gz",
                   "person.csv"
               ],
               "details": [
                   {
                       "create_time": 1547090015132,
                       "update_time": 1547090015132,
                       "resource_type": "jar",
                       "resource_name": "part-00000-9dfc17b1-2feb-45c5-b81d-bff533d6ed13.csv.gz",
                       "status": "READY",
                       "underlying_name": "db50c4dc-7187-4eb9-a5d0-73ba8102ea5e_part-00000-9dfc17b1-2feb-45c5-b81d-bff533d6ed13.csv.gz"
                   },
                   {
                       "create_time": 1547091098668,
                       "update_time": 1547091098668,
                       "resource_type": "file",
                       "resource_name": "person.csv",
                       "status": "READY",
                       "underlying_name": "a4243a8c-bca6-4e77-a968-1f3b00217474_person.csv"
                   }
               ],
               "create_time": 1547090015131,
               "update_time": 1547091098666
           }
       ],
       "modules": [
           {
               "module_name": "gatk",
               "status": "READY",
               "resources": [
                   "gatk.jar",
                   "tika-core-1.18.jar",
                   "s3fs-2.2.2.jar"
               ],
               "create_time": 1521532893736,
               "update_time": 1521552364503
           }
       ]
   }

Status Codes
------------

:ref:`Table 6 <dli_02_0168__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0168__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 6** Status codes

   =========== ========================
   Status Code Description
   =========== ========================
   200         The query is successful.
   400         Request error.
   500         Internal service error.
   =========== ========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
