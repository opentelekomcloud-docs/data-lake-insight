:original_name: dli_02_0258.html

.. _dli_02_0258:

Creating a Global Variable
==========================

Function
--------

This API is used to create a global variable.

URI
---

-  URI format

   POST /v1.0/{project_id}/variables

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

   +--------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter    | Mandatory | Type    | Description                                                                                                                                                                          |
   +==============+===========+=========+======================================================================================================================================================================================+
   | var_name     | Yes       | String  | A global variable name can contain a maximum of 128 characters, including only digits, letters, and underscores (_), but cannot start with an underscore (_) or contain only digits. |
   +--------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | var_value    | Yes       | String  | Global variable value.                                                                                                                                                               |
   +--------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | is_sensitive | No        | Boolean | Whether to set a variable as a sensitive variable. The default value is **false**.                                                                                                   |
   +--------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | Message content.                                                                                                            |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Create a global variable that is sensitive.

.. code-block::

   {
       "var_name": "string",
       "var_value": "string",
       "is_sensitive": true
   }

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "string"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0258__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0258__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         A variable is created successfully.
   400         The input parameter is invalid.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 5** Error codes

   ========== =============================
   Error Code Error Message
   ========== =============================
   DLI.0001   Parameter check errors occur.
   DLI.0999   The object exists.
   ========== =============================
