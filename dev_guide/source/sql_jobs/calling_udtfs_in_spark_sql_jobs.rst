:original_name: dli_09_0204.html

.. _dli_09_0204:

Calling UDTFs in Spark SQL Jobs
===============================

Scenario
--------

You can use Hive User-Defined Table-Generating Functions (UDTF) to customize table-valued functions. Hive UDTFs are used for the one-in-multiple-out data operations. UDTF reads a row of data and output multiple values.

Constraints
-----------

-  To perform UDTF-related operations on DLI, you need to create a SQL queue instead of using the default queue.

-  When UDTFs are used by multiple accounts, other users, except the user who creates them, need to be authorized before using the UDTF. The authorization operations are as follows:

   Log in to the DLI console and choose **Data Management** > **Package Management**. On the displayed page, select your UDTF Jar package and click **Manage Permissions** in the **Operation** column. On the permission management page, click **Grant Permission** in the upper right corner and select the required permissions.

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

The process of developing a UDTF is as follows:


.. figure:: /_static/images/en-us_image_0000001200075414.png
   :alt: **Figure 1** Development process

   **Figure 1** Development process

.. table:: **Table 2** Process description

   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+
   | No. | Phase                                                 | Software Portal | Description                                                                                                           |
   +=====+=======================================================+=================+=======================================================================================================================+
   | 1   | Create a Maven project and configure the POM file.    | IntelliJ IDEA   | Write UDTF code by referring the steps in :ref:`Procedure <dli_09_0204__en-us_topic_0206789796_section164701187527>`. |
   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+
   | 2   | Write UDTF code.                                      |                 |                                                                                                                       |
   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+
   | 3   | Debug, compile, and pack the code into a Jar package. |                 |                                                                                                                       |
   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+
   | 4   | Upload the Jar package to OBS.                        | OBS console     | Upload the UDTF Jar file to an OBS directory.                                                                         |
   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+
   | 5   | Create the UDTF on DLI.                               | DLI console     | Create a UDTF on the SQL job management page of the DLI console.                                                      |
   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+
   | 6   | Verify and use the UDTF on DLI.                       | DLI console     | Use the UDTF in your DLI job.                                                                                         |
   +-----+-------------------------------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+

.. _dli_09_0204__en-us_topic_0206789796_section164701187527:

Procedure
---------

#. Create a Maven project and configure the POM file. This step uses IntelliJ IDEA 2020.2 as an example.

   a. Start IntelliJ IDEA and choose **File** > **New** > **Project**.


      .. figure:: /_static/images/en-us_image_0000001245542509.png
         :alt: **Figure 2** Creating a project

         **Figure 2** Creating a project

   b. Choose **Maven**, set **Project SDK** to **1.8**, and click **Next**.


      .. figure:: /_static/images/en-us_image_0000001245010109.png
         :alt: **Figure 3** Choosing Maven

         **Figure 3** Choosing Maven

   c. Set the project name, configure the storage path, and click **Finish**.


      .. figure:: /_static/images/en-us_image_0000001245210469.png
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


      .. figure:: /_static/images/en-us_image_0000001245542693.png
         :alt: **Figure 5** Adding configurations to the POM file

         **Figure 5** Adding configurations to the POM file

   e. Choose **src** > **main** and right-click the **java** folder. Choose **New** > **Package** to create a package and a class file.


      .. figure:: /_static/images/en-us_image_0000001245011273.png
         :alt: **Figure 6** Creating a package and a class file

         **Figure 6** Creating a package and a class file

      Set the package name as you need. Then, press **Enter**.

      Create a Java Class file in the package path. In this example, the Java Class file is **UDTFSplit**.

#. Write UDTF code. For sample code, see :ref:`Sample Code <dli_09_0204__en-us_topic_0206789796_section10593204711240>`.

   The UDTF class must inherit **org.apache.hadoop.hive.ql.udf.generic.GenericUDTF** to implement the **initialize**, **process**, and **close** methods.

   a. Call the **initialize** method in the UDTF. This method returns the information about the returned data rows of the UDTF, such as the number and type.

   b. Call the **process** method to process data. Each time **forward()** is called in the **process** method, a row is generated.

      If multiple columns are generated, you can put the values in an array and pass the array to the **forward()** function.

      .. code-block::

         public void process(Object[] args) throws HiveException {
                 // TODO Auto-generated method stub
                 if(args.length == 0){
                     return;
                 }
                 String input = args[0].toString();
                 if(StringUtils.isEmpty(input)){
                     return;
                 }
                 String[] test = input.split(";");
                 for (int i = 0; i < test.length; i++) {
                     try {
                         String[] result = test[i].split(":");
                         forward(result);
                     } catch (Exception e) {
                         continue;
                     }
                 }

             }

   c. Call the **close** method to clear methods that need to be closed.

#. Use IntelliJ IDEA to compile the code and pack it into the JAR package.

   a. Click **Maven** in the tool bar on the right, and click **clean** and **compile** to compile the code.

      After the compilation is successful, click **package**.

      The generated JAR package is stored in the **target** directory. In this example, **MyUDTF-1.0-SNAPSHOT.jar** is stored in **D:\\MyUDTF\\target**.

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

#. .. _dli_09_0204__en-us_topic_0206789796_li9516133616203:

   Create the UDTF on DLI.

   a. Log in to the DLI console, choose **SQL Editor**. Set **Engine** to **spark**, and select the created SQL queue and database.

   b. In the SQL editing area, enter the path of the JAR file to be uploaded to create a UDTF and click **Execute**.

      .. code-block::

         CREATE FUNCTION mytestsplit AS 'com.demo.UDTFSplit' using jar 'obs://dli-test-obs01/MyUDTF-1.0-SNAPSHOT.jar';

#. Restart the original SQL queue for the added function to take effect.

   a. Log in to the DLI management console and choose **Resources** > **Queue Management** from the navigation pane. In the **Operation** column of the SQL queue job, click **Restart**.
   b. In the **Restart** dialog box, click **OK**.

#. Verify and use the UDTF on DLI.

   Use the UDTF created in :ref:`6 <dli_09_0204__en-us_topic_0206789796_li9516133616203>` in the SELECT statement as follows:

   .. code-block::

      select mytestsplit('abc:123\;efd:567\;utf:890');

#. (Optional) Delete the UDTF.

   If this function is no longer used, run the following statement to delete the function:

   .. code-block::

      Drop FUNCTION mytestsplit;

.. _dli_09_0204__en-us_topic_0206789796_section10593204711240:

Sample Code
-----------

The complete **UDTFSplit.java** code is as follows:

.. code-block::

   import java.util.ArrayList;

   import org.apache.commons.lang.StringUtils;
   import org.apache.hadoop.hive.ql.exec.UDFArgumentException;
   import org.apache.hadoop.hive.ql.exec.UDFArgumentLengthException;
   import org.apache.hadoop.hive.ql.metadata.HiveException;
   import org.apache.hadoop.hive.ql.udf.generic.GenericUDTF;
   import org.apache.hadoop.hive.serde2.objectinspector.ObjectInspector;
   import org.apache.hadoop.hive.serde2.objectinspector.ObjectInspectorFactory;
   import org.apache.hadoop.hive.serde2.objectinspector.StructObjectInspector;
   import org.apache.hadoop.hive.serde2.objectinspector.primitive.PrimitiveObjectInspectorFactory;

   public class UDTFSplit extends GenericUDTF {

       @Override
       public void close() throws HiveException {
           // TODO Auto-generated method stub

       }

       @Override
       public void process(Object[] args) throws HiveException {
           // TODO Auto-generated method stub
           if(args.length == 0){
               return;
           }
           String input = args[0].toString();
           if(StringUtils.isEmpty(input)){
               return;
           }
           String[] test = input.split(";");
           for (int i = 0; i < test.length; i++) {
               try {
                   String[] result = test[i].split(":");
                   forward(result);
               } catch (Exception e) {
                   continue;
               }
           }

       }

       @Override
       public StructObjectInspector initialize(ObjectInspector[] args) throws UDFArgumentException {
           if (args.length != 1) {
               throw new UDFArgumentLengthException("ExplodeMap takes only one argument");
           }
           if (args[0].getCategory() != ObjectInspector.Category.PRIMITIVE) {
               throw new UDFArgumentException("ExplodeMap takes string as a parameter");
           }

           ArrayList<String> fieldNames = new ArrayList<String>();
           ArrayList<ObjectInspector> fieldOIs = new ArrayList<ObjectInspector>();
           fieldNames.add("col1");
           fieldOIs.add(PrimitiveObjectInspectorFactory.javaStringObjectInspector);
           fieldNames.add("col2");
           fieldOIs.add(PrimitiveObjectInspectorFactory.javaStringObjectInspector);

           return ObjectInspectorFactory.getStandardStructObjectInspector(fieldNames, fieldOIs);
       }

   }
