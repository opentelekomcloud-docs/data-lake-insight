:original_name: dli_03_0263.html

.. _dli_03_0263:

Can a Member Account Use Global Variables Created by Other Member Accounts?
===========================================================================

No, a global variable can only be used by the user who created it. Global variables can be used to simplify complex parameters. For example, long and difficult variables can be replaced to improve the readability of SQL statements.

The restrictions on using global variables are as follows:

-  Existing sensitive variables can only be used by their respective creators. Other common global variables are shared by users under the same account and project.
-  If there are multiple global variables with the same name in the same project under an account, delete the redundant global variables to ensure that the global variables are unique in the same project. In this case, all users who have the permission to modify the global variables can change the variable values.
-  If there are multiple global variables with the same name in the same project under an account, delete the global variables created by the user first. If there are only unique global variables, all users who have the delete permission can delete the global variables.
