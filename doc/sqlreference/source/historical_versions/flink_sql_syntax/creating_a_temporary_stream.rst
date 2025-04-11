:original_name: dli_08_0258.html

.. _dli_08_0258:

Creating a Temporary Stream
===========================

Function
--------

The temporary stream is used to simplify SQL logic. If complex SQL logic is followed, write SQL statements concatenated with temporary streams. The temporary stream is just a logical concept and does not generate any data.

Syntax
------

The syntax for creating a temporary stream is as follows:

::

   CREATE TEMP STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )

Example
-------

The following is an example of creating a temporary stream:

::

   create temp stream a2(attr1 int, attr2 string);
