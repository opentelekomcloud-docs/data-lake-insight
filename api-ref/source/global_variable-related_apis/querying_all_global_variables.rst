:original_name: dli_02_0261.html

.. _dli_02_0261:

Querying All Global Variables
=============================

Function
--------

This API is used to query information about all global variables in the current project.

URI
---

-  URI format

   GET /v1.0/{project_id}/variables

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------+-----------+---------+----------------------------------------------------------------------------------+
      | Parameter | Mandatory | Type    | Description                                                                      |
      +===========+===========+=========+==================================================================================+
      | limit     | No        | Integer | Number of returned records displayed on each page. The default value is **100**. |
      +-----------+-----------+---------+----------------------------------------------------------------------------------+
      | offset    | No        | Integer | Offset. The default value is **0**.                                              |
      +-----------+-----------+---------+----------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type             | Description                                                                                                       |
   +=============+===========+==================+===================================================================================================================+
   | is_success  | No        | Boolean          | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | message     | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | count       | No        | Integer          | Number of global variables.                                                                                       |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | global_vars | No        | Array of objects | Global variable information. For details, see :ref:`Table 4 <dli_02_0261__table15129182752011>`.                  |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0261__table15129182752011:

.. table:: **Table 4** **global_vars** parameters

   +--------------+-----------+---------+----------------------------------------------------+
   | Parameter    | Mandatory | Type    | Description                                        |
   +==============+===========+=========+====================================================+
   | id           | No        | Long    | Global variable ID.                                |
   +--------------+-----------+---------+----------------------------------------------------+
   | var_name     | Yes       | String  | Global variable name.                              |
   +--------------+-----------+---------+----------------------------------------------------+
   | var_value    | Yes       | String  | Global variable value.                             |
   +--------------+-----------+---------+----------------------------------------------------+
   | project_id   | No        | String  | Project ID.                                        |
   +--------------+-----------+---------+----------------------------------------------------+
   | user_id      | No        | String  | User ID.                                           |
   +--------------+-----------+---------+----------------------------------------------------+
   | user_name    | No        | String  | Username                                           |
   +--------------+-----------+---------+----------------------------------------------------+
   | is_sensitive | No        | Boolean | Whether to set a variable as a sensitive variable. |
   +--------------+-----------+---------+----------------------------------------------------+
   | create_time  | No        | Long    | Creation time                                      |
   +--------------+-----------+---------+----------------------------------------------------+
   | update_time  | No        | Long    | Update time                                        |
   +--------------+-----------+---------+----------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "string",
       "count": 0,
       "global_vars": [
           {
               "id": 0,
               "var_name": "string",
               "var_value": "string",
               "project_id": "string",
               "user_id": "string"
           }
       ]
   }

Status Codes
------------

.. table:: **Table 5** Status codes

   =========== =======================================
   Status Code Description
   =========== =======================================
   200         All variables are queried successfully.
   400         The input parameter is invalid.
   =========== =======================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 6** Error codes

   ========== =============================
   Error Code Error Message
   ========== =============================
   DLI.0001   Parameter check errors occur.
   DLI.0999   Server-side errors occur.
   ========== =============================
