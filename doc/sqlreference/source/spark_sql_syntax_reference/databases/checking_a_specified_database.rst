:original_name: dli_08_0073.html

.. _dli_08_0073:

Checking a Specified Database
=============================

Function
--------

This syntax is used to view the information about a specified database, including the database name and database description.

Syntax
------

::

   DESCRIBE DATABASE [EXTENDED] db_name;

Keywords
--------

EXTENDED: Displays the database properties.

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Description                                                                                                                                          |
   +===========+======================================================================================================================================================+
   | db_name   | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_). |
   +-----------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

If the database to be viewed does not exist, the system reports an error.

Example
-------

#. Create a database, for example, **testdb**, by referring to :ref:`Example <dli_08_0071__en-us_topic_0114776165_en-us_topic_0093946907_se85f897bfc724638829c13a14150cab6>`.

#. Run the following statement to query information about the **testdb** database:

   ::

      DESCRIBE DATABASE testdb;
