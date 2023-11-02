:original_name: dli_02_0259.html

.. _dli_02_0259:

Deleting a Global Variable
==========================

Function
--------

This API is used to delete a global variable.

.. note::

   Only the user who creates a global variable can delete the variable.

URI
---

-  URI format

   DELETE /v1.0/{project_id}/variables/{var_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                                                          |
      +============+===========+========+======================================================================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`.                                        |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | var_name   | Yes       | String | A global variable name can contain a maximum of 128 characters, including only digits, letters, and underscores (_), but cannot start with an underscore (_) or contain only digits. |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Parameter description

   .. table:: **Table 2** Response parameters

      +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type    | Description                                                                                                       |
      +============+===========+=========+===================================================================================================================+
      | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
      +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
      | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
      +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "string"
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0259__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0259__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 3** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         A variable is deleted successfully.
   400         The input parameter is invalid.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 4** Error codes

   ========== =============================
   Error Code Error Message
   ========== =============================
   DLI.0001   Parameter check errors occur.
   DLI.0999   Server-side errors occur.
   ========== =============================
