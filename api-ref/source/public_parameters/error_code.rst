:original_name: dli_02_0056.html

.. _dli_02_0056:

Error Code
==========

If an error occurs in API calling, no result is returned. Identify the cause of error based on the error codes of each API. If an error occurs in API calling, HTTP status code 4\ *xx* or 5\ *xx* is returned. The response body contains the specific error code and information. If you are unable to identify the cause of an error, contact technical personnel and provide the error code so that we can help you solve the problem as soon as possible.

Format of an Error Response Body
--------------------------------

If an error occurs during API calling, the system returns an error code and a message to you. The following shows the format of an error response body:

.. code-block::

   {
       "error_msg": "The format of message is error",
       "error_code": "DLI.0001"
   }

In the preceding information, **error_code** is an error code, and **error_msg** describes the error.

.. table:: **Table 1** Exceptions

   +------------+----------------+---------------------------------------------------------------------------------+
   | Parameter  | Parameter Type | Description                                                                     |
   +============+================+=================================================================================+
   | error_code | String         | Error code. For details, see :ref:`Table 2 <dli_02_0056__table40141715145511>`. |
   +------------+----------------+---------------------------------------------------------------------------------+
   | error_msg  | String         | Error details.                                                                  |
   +------------+----------------+---------------------------------------------------------------------------------+

Error Code Description
----------------------

.. _dli_02_0056__table40141715145511:

.. table:: **Table 2** Error codes

   =========== ========== ====================================
   Status Code Error Code Error Message
   =========== ========== ====================================
   400         DLI.0001   Parameter check errors occur.
   400         DLI.0002   The object does not exist.
   400         DLI.0003   SQL permission verification fails.
   400         DLI.0004   SQL syntax parsing errors occur.
   400         DLI.0005   SQL semantics parsing errors occur.
   400         DLI.0006   The object exists.
   400         DLI.0007   The operation is not supported.
   400         DLI.0008   Metadata errors occur.
   400         DLI.0009   System restrictions.
   400         DLI.0011   The file permission check fails.
   400         DLI.0012   Resource objects are unavailable.
   401         DLI.0013   User authentication errors occur.
   401         DLI.0014   Service authentication errors occur.
   400         DLI.0015   Token parsing error.
   400         DLI.0016   The identity and role are incorrect.
   400         DLI.0018   Data conversion errors occur.
   400         DLI.0019   The task times out.
   400         DLI.0100   The result expires.
   404         DLI.0023   No related resources were found.
   400         DLI.0999   Server-side errors occur.
   400         DLI.1028   The quota is insufficient.
   =========== ========== ====================================

Example
-------

If no queue named **testqueue** exists, the following error message is displayed when you submit a job submission request:

.. code-block::

   {
     "error_code": "DLI.0002",
     "error_msg": "There is no queue named testqueue"
   }
