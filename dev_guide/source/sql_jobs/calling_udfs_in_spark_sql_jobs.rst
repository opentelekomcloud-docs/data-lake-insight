:original_name: dli_09_0171.html

.. _dli_09_0171:

Calling UDFs in Spark SQL Jobs
==============================

Scenario
--------

DLI allows you to use Hive user-defined functions (UDFs) to query data. UDFs take effect only on a single row of data and are applicable to inserting and deleting a single data record.

Constraints
-----------

-  To perform UDF-related operations on DLI, you need to create a SQL queue instead of using the default queue.

-  When UDFs are used across accounts, other users, except the user who creates them, need to be authorized before using the UDF. The authorization operations are as follows:

   Log in to the DLI console and choose **Data Management** > **Package Management**. On the displayed page, select your UDF Jar package and click **Manage Permissions** in the **Operation** column. On the permission management page, click **Grant Permission** in the upper right corner and select the required permissions.

-  If you use a static class or interface in a UDF, add **try catch** to capture exceptions. Otherwise, package conflicts may occur.

Environment Preparations
------------------------

Before you start, set up the development environment.

.. table:: **Table 1** Development environment

   +---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Item          | Description                                                                                                                                 |
   +===============+=============================================================================================================================================+
   | OS            | Windows 7 or later                                                                                                                          |
   +---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | JDK           | JDK 1.8.                                                                                                                                    |
   +---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | IntelliJ IDEA | This tool is used for application development. The version of the tool must be 2019.1 or other compatible versions.                         |
   +---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Maven         | Basic configurations of the development environment. Maven is used for project management throughout the lifecycle of software development. |
   +---------------+---------------------------------------------------------------------------------------------------------------------------------------------+

Development Process
-------------------

The process of developing a UDF is as follows:


.. figure:: /_static/images/en-us_image_0000001200327862.png
   :alt: **Figure 1** Development process

   **Figure 1** Development process

.. table:: **Table 2** Process description

   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | No. | Phase                                                 | Software Portal | Description                                                                                                          |
   +=====+=======================================================+=================+======================================================================================================================+
   | 1   | Create a Maven project and configure the POM file.    | IntelliJ IDEA   | Write UDF code by referring the steps in :ref:`Procedure <dli_09_0171__en-us_topic_0206789796_section164701187527>`. |
   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | 2   | Write UDF code.                                       |                 |                                                                                                                      |
   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | 3   | Debug, compile, and pack the code into a Jar package. |                 |                                                                                                                      |
   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | 4   | Upload the Jar package to OBS.                        | OBS console     | Upload the UDF Jar file to an OBS directory.                                                                         |
   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | 5   | Create the UDF on DLI.                                | DLI console     | Create a UDF on the SQL job management page of the DLI console.                                                      |
   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | 6   | Verify and use the UDF on DLI.                        | DLI console     | Use the UDF in your DLI job.                                                                                         |
   +-----+-------------------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+

.. _dli_09_0171__en-us_topic_0206789796_section164701187527:

Procedure
---------

#. Create a Maven project and configure the POM file. This step uses IntelliJ IDEA 2020.2 as an example.

   a. Start IntelliJ IDEA and choose **File** > **New** > **Project**.


      .. figure:: /_static/images/en-us_image_0000001245448995.png
         :alt: **Figure 2** Creating a project

         **Figure 2** Creating a project

   b. Choose **Maven**, set **Project SDK** to **1.8**, and click **Next**.


      .. figure:: /_static/images/en-us_image_0000001245660555.png
         :alt: **Figure 3** Choosing Maven

         **Figure 3** Choosing Maven

   c. Set the project name, configure the storage path, and click **Finish**.


      .. figure:: /_static/images/en-us_image_0000001245649477.png
         :alt: **Figure 4** Creating a project

         **Figure 4** Creating a project

   d. Add the following content to the **pom.xml** file.

      .. code-block::

         <dependencies>
                 <dependency>
                     <groupId>org.apache.hive</groupId>
                     <artifactId>hive-exec</artifactId>
                     <version>1.2.1</version>
                 </dependency>
         </dependencies>


      .. figure:: /_static/images/en-us_image_0000001200329970.png
         :alt: **Figure 5** Adding configurations to the POM file

         **Figure 5** Adding configurations to the POM file

   e. Choose **src** > **main** and right-click the **java** folder. Choose **New** > **Package** to create a package and a class file.


      .. figure:: /_static/images/en-us_image_0000001245651049.png
         :alt: **Figure 6** Creating a package and a class file

         **Figure 6** Creating a package and a class file

      Set the package name as you need. Then, press **Enter**.

      Create a Java Class file in the package path. In this example, the Java Class file is **SumUdfDemo**.

#. Write UDF code.

   a. The UDF must inherit **org.apache.hadoop.hive.ql.UDF**.
   b. You must implement the **evaluate** function, which can be reloaded.

   For details about how to implement the UDF, see the following sample code:

   .. code-block::

      package com.demo;
      import org.apache.hadoop.hive.ql.exec.UDF;
        public class SumUdfDemo extends UDF {
          public int evaluate(int a, int b) {
           return a + b;
        }
       }

#. Use IntelliJ IDEA to compile the code and pack it into the JAR package.

   a. Click **Maven** in the tool bar on the right, and click **clean** and **compile** to compile the code.

      After the compilation is successful, click **package**.

      The generated JAR package is stored in the **target** directory. In this example, **MyUDF-1.0-SNAPSHOT.jar** is stored in **D:\\DLITest\\MyUDF\\target**.

#. Log in to the OBS console and upload the file to the OBS path.

   .. note::

      The region of the OBS bucket to which the Jar package is uploaded must be the same as the region of the DLI queue. Cross-region operations are not allowed.

#. (Optional) Upload the file to DLI for package management.

   a. Log in to the DLI management console and choose **Data Management** > **Package Management**.
   b. On the **Package Management** page, click **Create** in the upper right corner.
   c. In the **Create Package** dialog, set the following parameters:

      #. **Type**: Select **JAR**.
      #. **OBS Path**: Specify the OBS path for storing the package.
      #. Set **Group** and **Group Name** as required for package identification and management.

   d. Click **OK**.

#. .. _dli_09_0171__en-us_topic_0206789796_li9516133616203:

   Create the UDF on DLI.

   a. Log in to the DLI console, choose **SQL Editor**. Set **Engine** to **spark**, and select the created SQL queue and database.

   b. In the SQL editing area, run the following statement to create a UDF and click **Execute**.

      .. code-block::

         CREATE FUNCTION TestSumUDF AS 'com.demo.SumUdfDemo' using jar 'obs://dli-test-obs01/MyUDF-1.0-SNAPSHOT.jar';

#. Restart the original SQL queue for the added function to take effect.

   a. Log in to the DLI console and choose **Queue Management** from the navigation pane. In the **Operation** column of the SQL queue job, click **Restart**.
   b. In the **Restart** dialog box, click **OK**.

#. Call the UDF.

   Use the UDF created in :ref:`6 <dli_09_0171__en-us_topic_0206789796_li9516133616203>` in the SELECT statement as follows:

   .. code-block::

      select TestSumUDF(1,2);

#. (Optional) Delete the UDF.

   If the UDF is no longer used, run the following statement to delete it:

   .. code-block::

      Drop FUNCTION TestSumUDF;
