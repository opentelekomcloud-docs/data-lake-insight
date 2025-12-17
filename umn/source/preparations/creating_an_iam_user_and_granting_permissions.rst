:original_name: dli_01_0418.html

.. _dli_01_0418:

Creating an IAM User and Granting Permissions
=============================================

You can use Identity and Access Management (IAM) to implement fine-grained permissions management for your DLI resources. For details, see :ref:`Overview <dli_01_0417>`.

If your cloud account does not need individual IAM users, then you may skip over this section.

This section describes how to create an IAM user and grant DLI permissions to the user. :ref:`Figure 1 <dli_01_0418__fig4118155455715>` shows the procedure.

Prerequisites
-------------

Before granting permissions to a user group, familiarize yourself with the DLI permissions that can be added to the user group and select them as needed.

Process Flow
------------

.. _dli_01_0418__fig4118155455715:

.. figure:: /_static/images/en-us_image_0000002415789969.png
   :alt: **Figure 1** Process for granting DLI permissions

   **Figure 1** Process for granting DLI permissions

.. table:: **Table 1** Procedure

   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No.                   | Step                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                       |
   +=======================+==============================================================+===================================================================================================================================================================================================================================================================================================================================================================================================+
   | 1                     | Create a user group and grant permissions to it.             | Create a user group on the IAM console and grant the **DLI ReadOnlyAccess** permission to it.                                                                                                                                                                                                                                                                                                     |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2                     | Create a user and add them to the user group.                | Create a user on the IAM console and add them to the created user group.                                                                                                                                                                                                                                                                                                                          |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3                     | Log in as the IAM user and verify permissions.               | Log in to the console using the newly created user, switch to the authorized region, and verify permissions.                                                                                                                                                                                                                                                                                      |
   |                       |                                                              |                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                                                              | -  Choose **Service List** > **Data Lake Insight**. If you can view the elastic resource pool list on the **Resources** > **Resource Pool** page but cannot purchase an elastic resource pool by clicking **Buy Resource Pool** in the upper right corner (assuming the current permission includes only **DLI ReadOnlyAccess**), the **DLI ReadOnlyAccess** permission has already taken effect. |
   |                       |                                                              | -  Choose any other service in **Service List**. If a message appears indicating that you have insufficient permissions to access the service, the **DLI ReadOnlyAccess** permission has already taken effect.                                                                                                                                                                                    |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4                     | Create a custom policy and associate it with the user group. | If the predefined DLI permissions in the system do not meet your authorization requirements, you can create custom policies.                                                                                                                                                                                                                                                                      |
   |                       |                                                              |                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                                                              | For details about how to create a custom policy, see :ref:`Creating a Custom Policy <dli_01_0451__en-us_topic_0206789937_section15041715821>`.                                                                                                                                                                                                                                                    |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
