:original_name: dli_spark_javahash.html

.. _dli_spark_javahash:

javahash
========

This function is used to return the hash value of **a**.

Syntax
------

.. code-block::

   javahash(string a)

Parameters
----------

.. table:: **Table 1** Parameter

   ========= ========= ====== ==========================================
   Parameter Mandatory Type   Description
   ========= ========= ====== ==========================================
   a         Yes       STRING Data whose hash value needs to be returned
   ========= ========= ====== ==========================================

Return Values
-------------

The return value is of the STRING type.

.. note::

   The hash value is returned. If the value of **a** is **null**, an error is reported.

Example Code
------------

The value **48690** is returned.

.. code-block::

   select javahash("123");

The value **123** is returned.

.. code-block::

   select javahash(123);
