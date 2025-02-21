:original_name: dli_01_0463.html

.. _dli_01_0463:

Adding Tags to a Flink Job
==========================

A tag is a key-value pair customized by users and used to identify cloud resources. It helps users to classify and search for cloud resources. A tag consists of a tag key and a tag value.

DLI allows you to add tags to Flink jobs. You can add tags to Flink jobs to identify information such as the project name, service type, and background. If you use tags in other cloud services, you are advised to create the same tag key-value pairs for cloud resources used by the same business to keep consistency.

DLI supports the following two types of tags:

-  Resource tags: indicate non-global tags created on DLI.

-  Predefined tags: global tags created on Tag Management Service (TMS).

This section includes the following content:

-  :ref:`Managing a Job Tag <dli_01_0463__section236374613167>`
-  :ref:`Searching for a Job by Tag <dli_01_0463__section911882919307>`

.. _dli_01_0463__section236374613167:

Managing a Job Tag
------------------

DLI allows you to add, modify, or delete tags for jobs.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.
#. Click the name of the job to be viewed. The **Job Details** page is displayed.
#. Click **Tags** to display the tag information about the current job.
#. Click **Add/Edit Tag** to open to the **Add/Edit Tag** dialog box.
#. Configure the tag parameters in the **Add/Edit Tag** dialog box.

   .. table:: **Table 1** Tag parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                               |
      +===================================+===========================================================================================================================================================================================================================================================================================================+
      | Tag key                           | You can perform the following operations:                                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Click the text box and select a predefined tag key from the drop-down list.                                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    To add a predefined tag, you need to create one on TMS and then select it from the **Tag key** drop-down list. You can click **View predefined tags** to go to the **Predefined Tags** page of the TMS console. Then, click **Create Tag** in the upper corner of the page to create a predefined tag. |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Enter a tag key in the text box.                                                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |       A tag key can contain a maximum of 128 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                        |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tag value                         | You can perform the following operations:                                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Click the text box and select a predefined tag value from the drop-down list.                                                                                                                                                                                                                          |
      |                                   | -  Enter a tag value in the text box.                                                                                                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |       A tag value can contain a maximum of 255 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed.                                                                                                                                                                |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  A maximum of 20 tags can be added.
      -  Only one tag value can be added to a tag key.
      -  The key name in each resource must be unique.

#. Click **OK**.
#. (Optional) In the tag list, locate the row where the tag you want to delete resides, click **Delete** in the **Operation** column to delete the tag.

.. _dli_01_0463__section911882919307:

Searching for a Job by Tag
--------------------------

If tags have been added to a job, you can search for the job by setting tag filtering conditions to quickly find it.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.
#. In the upper right corner of the page, click the search box and select **Tags**.
#. Choose a tag key and value as prompted. If no tag key or value is available, create a tag for the job. For details, see :ref:`Managing a Job Tag <dli_01_0463__section236374613167>`.
#. Choose other tags to generate a tag combination for job search.
#. Click search icon. The target job will be displayed in the job list.
