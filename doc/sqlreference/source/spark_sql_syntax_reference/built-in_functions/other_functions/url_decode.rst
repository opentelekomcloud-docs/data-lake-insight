:original_name: dli_spark_url_decode.html

.. _dli_spark_url_decode:

url_decode
==========

This function is used to convert a string from the **application/x-www-form-urlencoded MIME** format to regular characters.

Syntax
------

.. code-block::

   url_decode(string <input>[, string <encoding>])

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

   UTF-8-encoded string of the STRING type

Example Code
------------

The value **Example for URL_DECODE:// dsf (fasfs)** is returned.

.. code-block::

   select url_decode('Example+for+url_decode+%3A%2F%2F+dsf%28fasfs%29', 'GBK');
