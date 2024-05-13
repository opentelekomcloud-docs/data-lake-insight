:original_name: dli_02_0028.html

.. _dli_02_0028:

Creating a Database (Discarded)
===============================

Function
--------

This API is used to add a database.

.. note::

   This API has been discarded and is not recommended.

URI
---

-  URI format

   POST /v1.0/{project_id}/databases

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory       | Type             | Description                                                                                                                                 |
   +=======================+=================+==================+=============================================================================================================================================+
   | database_name         | Yes             | String           | Name of the created database.                                                                                                               |
   |                       |                 |                  |                                                                                                                                             |
   |                       |                 |                  | -  The database name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). |
   |                       |                 |                  | -  The database name is case insensitive and cannot be left blank.                                                                          |
   |                       |                 |                  | -  The length of the database name cannot exceed 128 characters.                                                                            |
   |                       |                 |                  |                                                                                                                                             |
   |                       |                 |                  | .. note::                                                                                                                                   |
   |                       |                 |                  |                                                                                                                                             |
   |                       |                 |                  |    The **default** database is a built-in database. You cannot create a database named **default**.                                         |
   +-----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | description           | No              | String           | Information about the created database.                                                                                                     |
   +-----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | enterprise_project_id | No              | String           | Enterprise project ID. The value **0** indicates the default enterprise project.                                                            |
   |                       |                 |                  |                                                                                                                                             |
   |                       |                 |                  | .. note::                                                                                                                                   |
   |                       |                 |                  |                                                                                                                                             |
   |                       |                 |                  |    Users who have enabled Enterprise Management can set this parameter to bind a specified project.                                         |
   +-----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | tags                  | No              | Array of objects | Database tag. For details, see :ref:`Table 3 <dli_02_0028__table9391124139>`.                                                               |
   +-----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0028__table9391124139:

.. table:: **Table 3** tags parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                     |
   +=================+=================+=================+=================================================================================================================================================================================================================+
   | Key             | Yes             | String          | Tag key.                                                                                                                                                                                                        |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 | .. note::                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 |    A tag key can contain a maximum of 128 characters. Only letters, digits, spaces, and special characters ``(_.:=+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Tag key.                                                                                                                                                                                                        |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 | .. note::                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 |    A tag value can contain a maximum of 255 characters. Only letters, digits, spaces, and special characters ``(_.:=+-@)`` are allowed. The value cannot start or end with a space.                             |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 4** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Create a test database named **db1**.

.. code-block::

   {
     "database_name": "db1",
     "description": "this is for test"
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

:ref:`Table 5 <dli_02_0028__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0028__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

   =========== ================================
   Status Code Description
   =========== ================================
   200         The job is created successfully.
   400         Request error.
   500         Internal service error.
   =========== ================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
