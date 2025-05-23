:original_name: dli_08_15087.html

.. _dli_08_15087:

Logical Functions
=================

.. table:: **Table 1** Logical functions

   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQL Function           | Return Type | Description                                                                                                                                                                  |
   +========================+=============+==============================================================================================================================================================================+
   | boolean1 OR boolean2   | BOOLEAN     | Returns **TRUE** if **boolean1** or **boolean2** is **TRUE**. Supports three-valued logic. For example, **true \|\| Null(BOOLEAN)** returns **TRUE**.                        |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean1 AND boolean2  | BOOLEAN     | Returns **TRUE** if both **boolean1** and **boolean2** are **TRUE**. Supports three-valued logic. For example, **true && Null(BOOLEAN)** returns **UNKNOWN**.                |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | NOT boolean            | BOOLEAN     | Returns **TRUE** if the **boolean** value is **FALSE**; returns **FALSE** if the **boolean** value is **TRUE**; returns **UNKNOWN** if the **boolean** value is **UNKNOWN**. |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean IS FALSE       | BOOLEAN     | Returns **TRUE** if the **boolean** value is **FALSE**; returns **FALSE** if **boolean** is **TRUE** or **UNKNOWN**.                                                         |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean IS NOT FALSE   | BOOLEAN     | Returns **TRUE** if **boolean** is **TRUE** or **UNKNOWN**; returns **FALSE** if **boolean** is **FALSE**.                                                                   |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean IS TRUE        | BOOLEAN     | Returns **TRUE** if **boolean** is **TRUE**; returns **FALSE** if **boolean** is **FALSE** or **UNKNOWN**.                                                                   |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean IS NOT TRUE    | BOOLEAN     | Returns **TRUE** if **boolean** is **FALSE** or **UNKNOWN**; returns **FALSE** if **boolean** is **TRUE**.                                                                   |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean IS UNKNOWN     | BOOLEAN     | Returns **TRUE** if the **boolean** value is **UNKNOWN**; returns **FALSE** if **boolean** is **TRUE** or **FALSE**.                                                         |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boolean IS NOT UNKNOWN | BOOLEAN     | Returns **TRUE** if **boolean** is **TRUE** or **FALSE**; returns **FALSE** if the **boolean** value is **UNKNOWN**.                                                         |
   +------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
