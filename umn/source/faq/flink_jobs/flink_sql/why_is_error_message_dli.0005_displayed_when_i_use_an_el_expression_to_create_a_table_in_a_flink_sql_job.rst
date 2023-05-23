:original_name: dli_03_0167.html

.. _dli_03_0167:

Why Is Error Message "DLI.0005" Displayed When I Use an EL Expression to Create a Table in a Flink SQL Job?
===========================================================================================================

Symptom
-------

When I run the creation statement with an EL expression in the table name in a Flink SQL job, the following error message is displayed:

.. code-block::

   DLI.0005: AnalysisException: t_user_message_input_#{date_format(date_sub(current_date(), 1), 'yyyymmddhhmmss')} is not a valid name for tables/databases. Valid names only contain alphabet characters, numbers and _.

Solution
--------

Replace the number sign (#) in the table name to the dollar sign ($). The format of the EL expression used in DLI should be **${**\ *expr*\ **}**.
