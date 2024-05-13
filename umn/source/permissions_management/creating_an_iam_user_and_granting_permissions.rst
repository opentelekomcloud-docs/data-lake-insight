:original_name: dli_01_0418.html

.. _dli_01_0418:

Creating an IAM User and Granting Permissions
=============================================

You can use Identity and Access Management (IAM) to implement fine-grained permissions control on DLI resources. For details, see :ref:`Overview <dli_01_0440>`.

If your cloud account does not need individual IAM users, then you may skip over this section.

This section describes how to create an IAM user and grant DLI permissions to the user. :ref:`Figure 1 <dli_01_0418__fig4118155455715>` shows the procedure.

Prerequisites
-------------

Before assigning permissions to user groups, you should learn about system policies and select the policies based on service requirements. For details about system permissions supported by DLI, see :ref:`DLI System Permissions <dli_01_0440__section6224422143120>`.

Process Flow
------------

.. _dli_01_0418__fig4118155455715:

.. figure:: /_static/images/en-us_image_0206789726.jpg
   :alt: **Figure 1** Process for granting DLI permissions

   **Figure 1** Process for granting DLI permissions

#. .. _dli_01_0418__li895020818018:

   Create a user group and grant the permission to it.

   Create a user group on the IAM console, and assign the **DLI ReadOnlyAccess** permission to the group.

#. Create a user and add the user to the user group.

   Create a user on the IAM console and add the user to the group created in :ref:`1 <dli_01_0418__li895020818018>`.

#. Log in and verify the permission.

   Log in to the management console using the newly created user, and verify that the user's permissions.

   -  Choose **Service List** > **Data Lake Insight**. The DLI management console is displayed. If you can view the queue list on the **Queue Management** page but cannot buy DLI queues by clicking **Buy Queue** in the upper right corner (assume that the current permission contains only **DLI ReadOnlyAccess**), the **DLI ReadOnlyAccess** permission has taken effect.
   -  Choose any other service in **Service List**. If a message appears indicating that you have insufficient permissions to access the service, the **DLI ReadOnlyAccess** permission has already taken effect.
