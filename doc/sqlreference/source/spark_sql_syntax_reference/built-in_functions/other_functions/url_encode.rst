:original_name: dli_spark_url_encode.html

.. _dli_spark_url_encode:

url_encode
==========

This function is used to encode a string in the **application/x-www-form-urlencoded MIME** format.

Syntax
------

url_encode(string <input>[, string <encoding>])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                       |
   +===========+===========+========+===================================================================================================================================================+
   | input     | Yes       | STRING | String to be entered                                                                                                                              |
   +-----------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | endcoding | No        | STRING | Encoding format. Standard encoding formats such as GBK and UTF-8 are supported. If this parameter is not specified, **UTF-8** is used by default. |
   +-----------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   If the value of **input** or **encoding** is **NULL**, **NULL** is returned.

Example Code
------------

The value **Example+for+url_encode+%3A%2F%2F+dsf%28fasfs%29** is returned.

.. code-block::

   select url_encode('Example for url_encode:// dsf(fasfs)', 'GBK');
