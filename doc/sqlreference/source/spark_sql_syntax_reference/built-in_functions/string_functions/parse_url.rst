:original_name: dli_spark_parse_url.html

.. _dli_spark_parse_url:

parse_url
=========

This character is used to return the specified part of a given URL. Valid values of **partToExtract** include **HOST**, **PATH**, **QUERY**, **REF**, **PROTOCOL**, **AUTHORITY**, **FILE**, and **USERINFO**.

For example, **parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'HOST')** returns **'facebook.com'**.

When the second parameter is set to **QUERY**, the third parameter can be used to extract the value of a specific parameter. For example, **parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'QUERY', 'k1')** returns **'v1'**.

Syntax
------

.. code-block::

   parse_url(string urlString, string partToExtract [, string keyToExtract])

Parameters
----------

.. table:: **Table 1** Parameters

   +---------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter     | Mandatory | Type   | Description                                                                                                                              |
   +===============+===========+========+==========================================================================================================================================+
   | urlString     | Yes       | STRING | URL. If the URL is invalid, an error is reported.                                                                                        |
   +---------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------+
   | partToExtract | Yes       | STRING | The value is case-insensitive and can be **HOST**, **PATH**, **QUERY**, **REF**, **PROTOCOL**, **AUTHORITY**, **FILE**, or **USERINFO**. |
   +---------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------+
   | keyToExtract  | No        | STRING | If the value of **partToExtract** is **QUERY**, the value is obtained based on the key.                                                  |
   +---------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type. The return rules are as follows:

.. note::

   -  If the value of **urlString**, **partToExtract**, or **keyToExtract** is **NULL**, **NULL** is returned.
   -  If the value of **partToExtract** does not meet requirements, an error is reported.

Example Code
------------

The value **example.com** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'HOST');

The value **/over/there/index.dtb** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'PATH');

The value **animal** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'QUERY', 'type');

The value **nose** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'REF');

The value **file** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'PROTOCOL');

The value **username@**\ *example*\ **.com:8042** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'AUTHORITY');

The value **username** is returned.

.. code-block::

   select parse_url('file://username@example.com:666/over/there/index.dtb?type=animal&name=narwhal#nose', 'USERINFO');
