:original_name: dli_08_0285.html

.. _dli_08_0285:

Displaying All Functions
========================

Function
--------

View all functions in the current project.

Syntax
------

::

   SHOW [USER|SYSTEM|ALL] FUNCTIONS ([LIKE] regex | [db_name.] function_name);

In the preceding statement, regex is a regular expression. For details about its parameters, see :ref:`Table 1 <dli_08_0285__table19771171316361>`.

.. _dli_08_0285__table19771171316361:

.. table:: **Table 1** Parameter examples

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Expression                        | Description                                                                                                                                                             |
   +===================================+=========================================================================================================================================================================+
   | 'xpath*'                          | Matches all functions whose names start with **xpath**.                                                                                                                 |
   |                                   |                                                                                                                                                                         |
   |                                   | Example: SHOW FUNCTIONS LIKE'xpath\* ;                                                                                                                                  |
   |                                   |                                                                                                                                                                         |
   |                                   | Matches functions whose names start with **xpath**, including **xpath**, **xpath_int**, and **xpath_string**.                                                           |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 'x[a-z]+'                         | Matches functions whose names start with **x** and is followed by one or more characters from a to z. For example, **xpath** and **xtest** can be matched.              |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 'x.*h'                            | Matches functions whose names start with **x**, end with **h**, and contain one or more characters in the middle. For example, **xpath** and **xtesth** can be matched. |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

For details about other expressions, see the official website.

Keywords
--------

LIKE: This qualifier is used only for compatibility and has no actual effect.

Precautions
-----------

The function that matches the given regular expression or function name are displayed. If no regular expression or name is provided, all functions are displayed. If USER or SYSTEM is specified, user-defined Spark SQL functions and system-defined Spark SQL functions are displayed, respectively.

Example
-------

This statement is used to view all functions.

::

   SHOW FUNCTIONS;
