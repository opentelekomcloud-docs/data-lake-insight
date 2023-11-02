:original_name: dli_02_0252.html

.. _dli_02_0252:

Viewing the Granted Permissions of a User
=========================================

Function
--------

This API is used to view the permissions granted to a user.

URI
---

-  URI format

   GET /v1.0/{project_id}/authorization/privileges

-  Parameter descriptions:

.. table:: **Table 1** URI parameters

   +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
   +============+===========+========+===============================================================================================================================================+
   | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
   +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

.. table:: **Table 2** query parameter description

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                          |
   +=================+=================+=================+======================================================================================================================+
   | object          | Yes             | String          | Data object to be assigned, which corresponds to the **object** in API permission assignment.                        |
   |                 |                 |                 |                                                                                                                      |
   |                 |                 |                 | -  **jobs.flink.\ Fink job ID**, data in the specified job will be queried.                                          |
   |                 |                 |                 | -  **groups. Package group name**, data in the specified package group will be queried.                              |
   |                 |                 |                 | -  **resources.\ Package name**, data in the specified package will be queried.                                      |
   |                 |                 |                 |                                                                                                                      |
   |                 |                 |                 |    .. note::                                                                                                         |
   |                 |                 |                 |                                                                                                                      |
   |                 |                 |                 |       When you view the packages in a group, the **object** format is **resources.package group name/package name**. |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | offset          | No              | Integer         | Specifies the offset of the page-based query.                                                                        |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | limit           | No              | Integer         | Number of records to be displayed of the page-based query.                                                           |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+

.. note::

   The following is an example of the URL containing the **query** parameter:

   GET /v1.0/{project_id}/authorization/privileges\ *?object={object}*

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type            | Description                                                                                                       |
   +=============+===========+=================+===================================================================================================================+
   | is_success  | Yes       | Boolean         | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | message     | Yes       | String          | Indicates the system prompt. If execution succeeds, this parameter may be left blank.                             |
   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | object_name | Yes       | String          | Object name.                                                                                                      |
   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | object_type | Yes       | String          | Object type.                                                                                                      |
   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | privileges  | No        | Array of Object | Permission information. For details, see :ref:`Table 4 <dli_02_0252__table431614366394>`.                         |
   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | count       | No        | Integer         | Total number of permissions.                                                                                      |
   +-------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0252__table431614366394:

.. table:: **Table 4** privileges parameters

   +------------+-----------+------------------+--------------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                                  |
   +============+===========+==================+==============================================================+
   | is_admin   | No        | Boolean          | Whether the database user is an administrator.               |
   +------------+-----------+------------------+--------------------------------------------------------------+
   | user_name  | No        | String           | Name of the user who has permission on the current database. |
   +------------+-----------+------------------+--------------------------------------------------------------+
   | privileges | No        | Array of Strings | Permission of the user on the database.                      |
   +------------+-----------+------------------+--------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "",
       "object_name": "9561",
       "object_type": "flink",
       "count": 2,
       "privileges": [
           {
               "user_name": "testuser1",
               "is_admin": true,
               "privileges": [
                   "ALL"
               ]
           },
           {
               "user_name": "user1",
               "is_admin": false,
               "privileges": [
                   "GET"
               ]
           }
       ]
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0252__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0252__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Authorization succeeds.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 6** Error codes

   +------------+-----------------------------------------------------------------------------+
   | Error Code | Error Message                                                               |
   +============+=============================================================================+
   | DLI.0001   | user input validation failed, object_type sql or saprk is not supported now |
   +------------+-----------------------------------------------------------------------------+
