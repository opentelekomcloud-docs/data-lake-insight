:original_name: dli_03_0066.html

.. _dli_03_0066:

Why Is Error "SQL_ANALYSIS_ERROR: Reference 't.id' is ambiguous, could be: t.id, t.id.;" Displayed When Two Tables Are Joined?
==============================================================================================================================

This message indicates that the two tables to be joined contain the same column, but the owner of the column is not specified when the command is executed.

For example, tables tb1 and tb2 contain the **id** field.

Incorrect command:

.. code-block::

   select id from tb1 join tb2;

Correct command:

.. code-block::

   select tb1.id from tb1 join tb2;
