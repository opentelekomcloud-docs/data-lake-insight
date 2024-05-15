:original_name: dli_05_0062.html

.. _dli_05_0062:

Calling UDAFs in Spark SQL Jobs
===============================

Scenario
--------

DLI allows you to use a Hive User Defined Aggregation Function (UDAF) to process multiple rows of data. Hive UDAF is usually used together with groupBy. It is equivalent to SUM() and AVG() commonly used in SQL and is also an aggregation function.

Constraints
-----------

-  To perform UDAF-related operations on DLI, you need to create a SQL queue instead of using the default queue.

-  When UDAFs are used across accounts, other users, except the user who creates them, need to be authorized before using the UDAF.

   To grant required permissions, log in to the DLI console and choose **Data Management** > **Package Management**. On the displayed page, select your UDAF Jar package and click **Manage Permissions** in the **Operation** column. On the permission management page, click **Grant Permission** in the upper right corner and select the required permissions.

-  If you use a static class or interface in a UDF, add **try catch** to capture exceptions. Otherwise, package conflicts may occur.

Environment Preparations
------------------------

Before you start, set up the development environment.

.. table:: **Table 1** Development environment

   +---------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item          | Description                                                                                                                                                                                                                                                                                                                        |
   +===============+====================================================================================================================================================================================================================================================================================================================================+
   | OS            | Windows 7 or later                                                                                                                                                                                                                                                                                                                 |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | JDK           | JDK 1.8 (`Java downloads <https://www.oracle.com/java/technologies/javase-downloads.html>`__).                                                                                                                                                                                                                                     |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | IntelliJ IDEA | `IntelliJ IDEA <https://www.jetbrains.com/idea/>`__ is used for application development. The version of the tool must be 2019.1 or later.                                                                                                                                                                                          |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Maven         | Basic configuration of the development environment. For details about how to get started, see `Downloading Apache Maven <https://maven.apache.org/download.cgi>`__ and `Installing Apache Maven <https://maven.apache.org/install.html>`__. Maven is used for project management throughout the lifecycle of software development. |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Development Process
-------------------

The following figure shows the process of developing a UDAF.


.. figure:: /_static/images/en-us_image_0000001487274748.png
   :alt: **Figure 1** Development process

   **Figure 1** Development process

.. table:: **Table 2** Process description

   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | No. | Phase                                                 | Software Portal | Description                                                                                                                                       |
   +=====+=======================================================+=================+===================================================================================================================================================+
   | 1   | Create a Maven project and configure the POM file.    | IntelliJ IDEA   | Compile the UDAF function code by referring to the :ref:`Procedure <dli_05_0062__en-us_topic_0000001488009165_section1255957113015>` description. |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2   | Editing UDAF code                                     |                 |                                                                                                                                                   |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3   | Debug, compile, and pack the code into a Jar package. |                 |                                                                                                                                                   |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4   | Upload the Jar package to OBS.                        | OBS console     | Upload the UDAF Jar file to an OBS path.                                                                                                          |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | 5   | Create a DLI package.                                 | DLI console     | Select the UDAF Jar file that has been uploaded to OBS for management.                                                                            |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | 6   | Create a UDAF on DLI.                                 | DLI console     | Create a UDAF on the SQL job management page of the DLI console.                                                                                  |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | 7   | Verify and use the UDAF.                              | DLI console     | Use the UDAF in your DLI job.                                                                                                                     |
   +-----+-------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_05_0062__en-us_topic_0000001488009165_section1255957113015:

Procedure
---------

#. Create a Maven project and configure the POM file. This step uses IntelliJ IDEA 2020.2 as an example.

   a. Start IntelliJ IDEA and choose **File** > **New** > **Project**.


      .. figure:: /_static/images/en-us_image_0000001538394645.png
         :alt: **Figure 2** Creating a project

         **Figure 2** Creating a project

   b. Choose **Maven**, set **Project SDK** to **1.8**, and click **Next**.


      .. figure:: /_static/images/en-us_image_0000001487114864.png
         :alt: **Figure 3** Configuring the project SDK

         **Figure 3** Configuring the project SDK

   c. Specify the project name and the project path, and click **Create**. In the displayed page, click **Finish**.


      .. figure:: /_static/images/en-us_image_0000001538514573.png
         :alt: **Figure 4** Setting project information

         **Figure 4** Setting project information

   d. Add the following content to the **pom.xml** file.

      .. code-block::

         <dependencies>
                  <dependency>
                      <groupId>org.apache.hive</groupId>
                      <artifactId>hive-exec</artifactId>
                      <version>1.2.1</version>
                  </dependency>
          </dependencies>


      .. figure:: /_static/images/en-us_image_0000001487434660.png
         :alt: **Figure 5** Adding configurations to the POM file

         **Figure 5** Adding configurations to the POM file

   e. Choose **src** > **main** and right-click the **java** folder. Choose **New** > **Package** to create a package and a class file.

      Set **Package** as required. In this example, set **Package** to **com.dli.demo**.


      .. figure:: /_static/images/en-us_image_0000001487594580.png
         :alt: **Figure 6** Creating a package

         **Figure 6** Creating a package

      Create a Java Class file in the package path. In this example, the Java Class file is **AvgFilterUDAFDemo**.


      .. figure:: /_static/images/en-us_image_0000001538354737.png
         :alt: **Figure 7** Creating a class

         **Figure 7** Creating a class

#. Write UDAF code. Pay attention to the following requirements when you implement the UDAF:

   -  The UDAF class must inherit from **org.apache.hadoop.hive.ql.exec.UDAF** and **org.apache.hadoop.hive.ql.exec.UDAFEvaluator** classes. The function class must inherit from the UDAF class, and the **Evaluator** class must implement the **UDAFEvaluator** interface.

   -  The **Evaluator** class must implement the **init**, **iterate**, **terminatePartial**, **merge**, and **terminate** functions of **UDAFEvaluator**.

      -  The **init** function overrides the **init** function of the **UDAFEvaluator** interface.
      -  The **iterate** function receives input parameters for internal iteration.
      -  The **terminatePartial** function has no parameter. It returns the data obtained after the **iterate** traversal is complete. **terminatePartial** is similar to Hadoop **Combiner**.
      -  The **merge** function receives the return values of **terminatePartial**.
      -  The **terminate** function returns the aggregated result.

      For details about how to implement the UDAF, see the following sample code:

      .. code-block::

         package com.dli.demo;

         import org.apache.hadoop.hive.ql.exec.UDAF;
         import org.apache.hadoop.hive.ql.exec.UDAFEvaluator;

         /***
          * @jdk jdk1.8.0
          * @version 1.0
          ***/
         public class AvgFilterUDAFDemo extends UDAF {

             /**
              * Defines the static inner class AvgFilter.
              */
             public static class PartialResult
             {
                 public Long sum;
             }

             public static class VarianceEvaluator implements UDAFEvaluator {

                 // Initializes the PartialResult object.
                 private AvgFilterUDAFDemo.PartialResult partial;

                 // Declares a VarianceEvaluator constructor that has no parameters.
                 public VarianceEvaluator(){

                     this.partial = new AvgFilterUDAFDemo.PartialResult();

                     init();
                 }

                 /**
                  * Initializes the UDAF, which is similar to a constructor.
                  */
                 @Override
                 public void init() {

                     // Sets the initial value of sum.
                     this.partial.sum = 0L;
                 }

                 /**
                  * Receives input parameters for internal iteration.
                  * @param x
                  * @return
                  */
                 public void iterate(Long x) {
                     if (x == null) {
                         return;
                     }
                     AvgFilterUDAFDemo.PartialResult tmp9_6 = this.partial;
                     tmp9_6.sum = tmp9_6.sum | x;
                 }

                 /**
                  * Returns the data obtained after the iterate traversal is complete.
                  * terminatePartial is similar to Hadoop Combiner.
                  * @return
                  */
                 public AvgFilterUDAFDemo.PartialResult terminatePartial()
                 {
                     return this.partial;
                 }

                 /**
                  * Receives the return values of terminatePartial and merges the data.
                  * @param
                  * @return
                  */
                 public void merge(AvgFilterUDAFDemo.PartialResult pr)
                 {
                     if (pr == null) {
                         return;
                     }
                     AvgFilterUDAFDemo.PartialResult tmp9_6 = this.partial;
                     tmp9_6.sum = tmp9_6.sum | pr.sum;
                 }

                 /**
                  * Returns the aggregated result.
                  * @return
                  */
                 public Long terminate()
                 {
                     if (this.partial.sum == null) {
                         return 0L;
                     }
                     return this.partial.sum;
                 }
             }
         }

#. Use IntelliJ IDEA to compile the code and pack it into the JAR package.

   a. Click **Maven** in the tool bar on the right, and click **clean** and **compile** to compile the code.

      After the compilation is successful, click **package**.


      .. figure:: /_static/images/en-us_image_0000001538394649.png
         :alt: **Figure 8** Exporting the Jar file

         **Figure 8** Exporting the Jar file

   b. The generated JAR package is stored in the **target** directory. In this example, **MyUDAF-1.0-SNAPSHOT.jar** is stored in **D:\\DLITest\\MyUDAF\\target**.

#. Log in to the OBS console and upload the file to the OBS path.

   .. note::

      The region of the OBS bucket to which the Jar package is uploaded must be the same as the region of the DLI queue. Cross-region operations are not allowed.

#. (Optional) Upload the file to DLI for package management.

   a. Log in to the DLI management console and choose **Data Management** > **Package Management**.
   b. On the **Package Management** page, click **Create** in the upper right corner.
   c. In the **Create Package** dialog, set the following parameters:

      -  **Type**: Select **JAR**.
      -  **OBS Path**: Specify the OBS path for storing the package.
      -  Set **Group** and **Group Name** as required for package identification and management.

   d. Click **OK**.

#. .. _dli_05_0062__en-us_topic_0000001488009165_li13507814611:

   Create the UDAF on DLI.

   a. Log in to the DLI management console and create a SQL queue and a database.

   b. Log in to the DLI console, choose **SQL Editor**. Set **Engine** to **spark**, and select the created SQL queue and database.

   c. In the SQL editing area, run the following statement to create a UDAF and click **Execute**.

      .. note::

         If the reloading function of the UDAF is enabled, the create statement changes.

      .. code-block::

         CREATE FUNCTION AvgFilterUDAFDemo AS 'com.dli.demo.AvgFilterUDAFDemo' using jar 'obs://dli-test-obs01/MyUDAF-1.0-SNAPSHOT.jar';

      Or

      .. code-block::

         CREATE OR REPLACE FUNCTION AvgFilterUDAFDemo AS 'com.dli.demo.AvgFilterUDAFDemo' using jar 'obs://dli-test-obs01/MyUDAF-1.0-SNAPSHOT.jar';

#. Restart the original SQL queue for the added function to take effect.

   a. Log in to the DLI management console and choose **Resources** > **Queue Management** from the navigation pane. In the **Operation** column of the SQL queue, click **Restart**.
   b. In the **Restart** dialog box, click **OK**.

#. Use the UDAF.

   Use the UDAF function created in :ref:`6 <dli_05_0062__en-us_topic_0000001488009165_li13507814611>` in the query statement:

   .. code-block::

      select AvgFilterUDAFDemo(real_stock_rate) AS show_rate FROM dw_ad_estimate_real_stock_rate limit 1000;

#. (Optional) Delete the UDAF.

   If the UDAF is no longer used, run the following statement to delete it:

   .. code-block::

      Drop FUNCTION AvgFilterUDAFDemo;
