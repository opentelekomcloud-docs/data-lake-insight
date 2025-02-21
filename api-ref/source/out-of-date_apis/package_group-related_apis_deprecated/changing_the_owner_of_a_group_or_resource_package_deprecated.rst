:original_name: dli_02_0253.html

.. _dli_02_0253:

Changing the Owner of a Group or Resource Package (Deprecated)
==============================================================

Function
--------

This API is used to change the owner of a program package.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   PUT /v2.0/{project_id}/resources/owner

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

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                 |
   +=================+=================+=================+=============================================================================================================================================================================================================================================+
   | new_owner       | Yes             | String          | New username. The name contains 5 to 32 characters, including only digits, letters, underscores (_), and hyphens (-). It cannot start with a digit.                                                                                         |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_name      | Yes             | String          | Group name. The name contains a maximum of 64 characters. Only digits, letters, periods (.), underscores (_), and hyphens (-) are allowed.                                                                                                  |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_name   | No              | String          | Package name. The name can contain only digits, letters, underscores (_), exclamation marks (!), hyphens (-), and periods (.), but cannot start with a period. The length (including the file name extension) cannot exceed 128 characters. |
   |                 |                 |                 |                                                                                                                                                                                                                                             |
   |                 |                 |                 | **This parameter is mandatory if you want to change the owner of a resource package in a group.**                                                                                                                                           |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   **group_name** and **resource_name** can be used independently or together.

   -  To change the owner of a group, use **group_name**.
   -  To change the owner of a resource package, use **resource_name**.
   -  To change the owner of a resource package in a group, use **group_name** and **resource_name** at the same time.

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Change the group name of the program package to **groupName** and the user name to **scuser1**.

.. code-block::

   {
       "new_owner": "scuser1",
       "group_name": "groupName"
   }

Example Response
----------------

.. code-block::

   {
       "is_success": "true",
       "message": ""
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0253__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0253__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ===========================================
   Status Code Description
   =========== ===========================================
   200         The modification operations are successful.
   404         Request error.
   =========== ===========================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 5** Error codes

   ========== ==============================
   Error Code Error Message
   ========== ==============================
   DLI.0002   No such user. userName:ssssss.
   ========== ==============================
