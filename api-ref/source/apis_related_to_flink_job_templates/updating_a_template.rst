:original_name: dli_02_0246.html

.. _dli_02_0246:

Updating a Template
===================

Function
--------

This API is used to update existing templates in DLI.

URI
---

-  URI format

   PUT /v1.0/{project_id}/streaming/job-templates/{template_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +-------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                                                                   |
      +=============+===========+========+===============================================================================================================================================+
      | project_id  | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +-------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | template_id | Yes       | String | Template ID.                                                                                                                                  |
      +-------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                  |
   +===========+===========+========+==============================================================================================================================================+
   | name      | No        | String | Template name. Length range: 0 to 57 characters.                                                                                             |
   +-----------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | desc      | No        | String | Template description. Length range: 0 to 512 characters.                                                                                     |
   +-----------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | sql_body  | No        | String | Stream SQL statement, which includes at least the following three parts: source, query, and sink. Length range: 0 to 1024 x 1024 characters. |
   +-----------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+

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

Update job template information, including the template name, template description, and template SQL statements.

.. code-block::

   {
       "name": "simple_stream_sql",
       "desc": "Example of quick start",
       "sql_body": "select * from source_table"
   }

Example Response
----------------

.. code-block::

   {
       "is_success": "true",
       "message": "The template is updated successfully.",
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0246__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0246__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         A template is updated successfully.
   400         The input parameter is invalid.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
