:original_name: dli_08_0356.html

.. _dli_08_0356:

string_split
============

The **string_split** function splits a target string into substrings based on the specified separator and returns a substring list.

Description
-----------

.. code-block::

   string_split(target, separator)

.. table:: **Table 1** string_split parameters

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                                                       |
   +=======================+=======================+===================================================================================================================+
   | target                | STRING                | Target string to be processed                                                                                     |
   |                       |                       |                                                                                                                   |
   |                       |                       | .. note::                                                                                                         |
   |                       |                       |                                                                                                                   |
   |                       |                       |    -  If **target** is **NULL**, an empty line is returned.                                                       |
   |                       |                       |    -  If **target** contains two or more consecutive separators, an empty substring is returned.                  |
   |                       |                       |    -  If **target** does not contain a specified separator, the original string passed to **target** is returned. |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------+
   | separator             | VARCHAR               | Delimiter. Currently, only single-character delimiters are supported.                                             |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------+

Example
-------

#. Prepare test input data.

   .. table:: **Table 2** Source table disSource

      ================ ===================
      target (STRING)  separator (VARCHAR)
      ================ ===================
      test-flink       ``-``
      flink            ``-``
      one-two-ww-three ``-``
      ================ ===================

#. Write test SQL statements.

   .. code-block::

      create table disSource(
        target STRING,
        separator  VARCHAR
      ) with (
        "connector.type" = "dis",
        "connector.region" = "xxx",
        "connector.channel" = "ygj-dis-in",
        "format.type" = 'csv'
      );

      create table disSink(
        target STRING,
        item STRING
      ) with (
        'connector.type' = 'dis',
        'connector.region' = 'xxx',
        'connector.channel' = 'ygj-dis-out',
        'format.type' = 'csv'
      );

      insert into
        disSink
      select
        target,
        item
      from
        disSource,
      lateral table(string_split(target, separator)) as T(item);

#. Check test results.

   .. table:: **Table 3** disSink result table

      ================ =============
      target (STRING)  item (STRING)
      ================ =============
      test-flink       test
      test-flink       flink
      flink            flink
      one-two-ww-three one
      one-two-ww-three two
      one-two-ww-three ww
      one-two-ww-three three
      ================ =============
