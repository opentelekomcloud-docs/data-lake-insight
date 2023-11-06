:original_name: dli_03_0140.html

.. _dli_03_0140:

Why Does My Account Have Insufficient Permissions Due to Arrears?
=================================================================

When you submit a job, a message is displayed indicating that the job fails to be submitted due to insufficient permission caused by arrears. In this case, you need to check the roles in your token:

-  **op_restrict**: The account permission is restricted due to insufficient balance. If your account balance is insufficient, the tokens of all online users under this account are revoked. If a user logs in to the system again, the **op_restrict** permission is added to the obtained token.
-  **op_suspended**: Your account is suspended due to arrears or other reasons. If your account is in arrears, the tokens of all online users under this account are revoked. If a user logs in to the system again, the **op_suspended** permission is added to the obtained token, and user operations (excluding cloud service users) will be restricted.

If the two roles described about are in your token, user operations are restricted.
