:original_name: dli_02_0029.html

.. _dli_02_0029:

Querying All Databases (Deprecated)
===================================

Function
--------

This API is used to query the information about all the databases.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                     |
      +=================+=================+=================+=================================================================================================================================================================================+
      | with-priv       | No              | Boolean         | Specifies whether to display the permission information. The value can be **true** or **false**. The default value is **false**.                                                |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | offset          | No              | Integer         | The value should be no less than **0**. The default value is **0**.                                                                                                             |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | limit           | No              | Integer         | Number of returned data records. The value must be greater than or equal to **0**. By default, all data records are returned.                                                   |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | keyword         | No              | String          | Database name filtering keyword. Fuzzy match is used to obtain all databases whose names contain the keyword.                                                                   |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | tags            | No              | String          | Database tags. The format is **key=value**.                                                                                                                                     |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | -  Request with one specified tag                                                                                                                                               |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | GET /v1.0/{project_id}/databases?offset=0&limit=10&with-priv=true&tags=k1%3Dv1                                                                                                  |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | The equal sign (=) is escaped to **%3D**, **k1** indicates the tag key, and **v1** indicates the tag value.                                                                     |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | -  Request with more than one tag                                                                                                                                               |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | Use commas (,) to separate tags. The commas (,) must be escaped to **%2C**. For example:                                                                                        |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | GET /v1.0/{project_id}/databases?offset=0&limit=10&with-priv=true&tags=k1%3Dv1%2Ck2%3Dv2                                                                                        |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | The equal sign (=) is escaped to **%3D**. **k1** indicates a tag key, and **v1** indicates the tag value. **k2** indicates another tag key, and **v2** indicates the tag value. |
      |                 |                 |                 |                                                                                                                                                                                 |
      |                 |                 |                 | Currently, only fuzzy query is supported. Exact query is not supported.                                                                                                         |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      GET /v1.0/{project_id}/databases?with-priv=\ *{is_with_priv}*\ &offset=\ *{offsetValue}*\ &limit=\ *{limitValue}*\ &keyword=\ *{keywordValue}*?tags=\ *{tagsValue}*

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type             | Description                                                                                                                 |
   +================+===========+==================+=============================================================================================================================+
   | is_success     | No        | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message        | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | database_count | No        | Integer          | Total number of databases.                                                                                                  |
   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | databases      | No        | Array of objects | Database information. For details, see :ref:`Table 4 <dli_02_0029__table119519521616>`.                                     |
   +----------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0029__table119519521616:

.. table:: **Table 4** **databases** parameters

   +-----------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory       | Type            | Description                                                                                         |
   +=======================+=================+=================+=====================================================================================================+
   | database_name         | No              | String          | Name of a database.                                                                                 |
   +-----------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------+
   | owner                 | No              | String          | Creator of a database.                                                                              |
   +-----------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------+
   | table_number          | No              | Integer         | Number of tables in a database.                                                                     |
   +-----------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------+
   | description           | No              | String          | Information about a database.                                                                       |
   +-----------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------+
   | enterprise_project_id | Yes             | String          | Enterprise project ID. The value **0** indicates the default enterprise project.                    |
   |                       |                 |                 |                                                                                                     |
   |                       |                 |                 | .. note::                                                                                           |
   |                       |                 |                 |                                                                                                     |
   |                       |                 |                 |    Users who have enabled Enterprise Management can set this parameter to bind a specified project. |
   +-----------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "database_count": 1,
     "databases": [
       {
         "database_name": "db2",
         "description": "this is for test",
         "owner": "tenant1",
         "table_number": 15

       }
     ]
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0029__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0029__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

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
