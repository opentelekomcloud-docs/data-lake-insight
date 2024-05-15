:original_name: dli_09_0127.html

.. _dli_09_0127:

Submitting a Job Using JDBC
===========================

Scenario
--------

In Linux or Windows, you can connect to the DLI server using JDBC.

.. note::

   -  Jobs submitted to DLI using JDBC are executed on the Spark engine.
   -  Once JDBC 2.X has undergone function reconstruction, query results can only be accessed from DLI job buckets. To utilize this feature, certain conditions must be met:

      -  On the DLI management console, choose **Global Configuration** > **Project** to configure the job bucket.
      -  Submit a service ticket to request the whitelisting of the feature that allows writing query results to buckets.

DLI supports 13 data types. Each type can be mapped to a JDBC type. If JDBC is used to connect to the server, you must use the mapped Java type. :ref:`Table 1 <dli_09_0127__en-us_topic_0093946957_table74891852105113>` describes the mapping relationships.

.. _dli_09_0127__en-us_topic_0093946957_table74891852105113:

.. table:: **Table 1** Data type mapping

   ============== ========= ====================
   DLI Data Type  JDBC Type Java Type
   ============== ========= ====================
   INT            INTEGER   java.lang.Integer
   STRING         VARCHAR   java.lang.String
   FLOAT          FLOAT     java.lang.Float
   DOUBLE         DOUBLE    java.lang.Double
   DECIMAL        DECIMAL   java.math.BigDecimal
   BOOLEAN        BOOLEAN   java.lang.Boolean
   SMALLINT/SHORT SMALLINT  java.lang.Short
   TINYINT        TINYINT   java.lang.Short
   BIGINT/LONG    BIGINT    java.lang.Long
   TIMESTAMP      TIMESTAMP java.sql.Timestamp
   CHAR           CHAR      Java.lang.Character
   VARCHAR        VARCHAR   java.lang.String
   DATE           DATE      java.sql.Date
   ============== ========= ====================

Prerequisites
-------------

Before using JDBC, perform the following operations:

#. Getting authorized.

   DLI uses the Identity and Access Management (IAM) to implement fine-grained permissions for your enterprise-level tenants. IAM provides identity authentication, permissions management, and access control, helping you securely access your resources.

   With IAM, you can use your account to create IAM users for your employees, and assign permissions to the users to control their access to specific resource types.

   Currently, roles (coarse-grained authorization) and policies (fine-grained authorization) are supported.

#. Create a queue. Choose **Resources** > **Queue Management**. On the page displayed, click **Buy Queue** in the upper right corner. On the **Buy Queue** page displayed, select **For general purpose** for **Type**, that is, the compute resources of the Spark job.

   .. note::

      If the user who creates the queue is not an administrator, the queue can be used only after being authorized by the administrator. For details about how to assign permissions, see .

Procedure
---------

#. Install JDK 1.7 or later on the computer where JDBC is installed, and configure environment variables.

#. Obtain the DLI JDBC driver package **dli-jdbc-<version>.zip** by referring to :ref:`Downloading the JDBC Driver Package <dli_09_0125>`. Decompress the package to obtain **dli-jdbc-<version>-jar-with-dependencies.jar**.

#. On the computer using JDBC, add **dli-jdbc-1.1.1-jar-with-dependencies.jar** to the **classpath** path of the Java project.

#. DLI JDBC provides two authentication modes, namely, token and AK/SK, to connect to DLI. For details about how to obtain the token and AK/SK, see :ref:`Performing Authentication <dli_09_0121>`.

#. Run the **Class.forName()** command to load the DLI JDBC driver.

   **Class.forName("com.dli.jdbc.DliDriver");**

#. Call the GetConnection method of DriverManager to create a connection.

   **Connection conn = DriverManager.getConnection(String url, Properties info);**

   JDBC configuration items are passed using the URL. For details, see :ref:`Table 2 <dli_09_0127__en-us_topic_0093946957_table7064698105347>`. JDBC configuration items can be separated by semicolons (;) in the URL, or you can dynamically set the attribute items using the Info object. For details, see :ref:`Table 3 <dli_09_0127__en-us_topic_0093946957_table31567601152339>`.

   .. _dli_09_0127__en-us_topic_0093946957_table7064698105347:

   .. table:: **Table 2** Database connection parameters

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                 |
      +===================================+=============================================================================================================================================================================================================================================================+
      | url                               | The URL format is as follows:                                                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                             |
      |                                   | jdbc:dli://<endPoint>/projectId? <key1>=<val1>;<key2>=<val2>...                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                             |
      |                                   | -  **EndPoint** indicates the DLI domain name. **ProjectId** indicates the project ID.                                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                             |
      |                                   |    To obtain the endpoint corresponding to DLI, contact the administrator to obtain the region and endpoint information. To obtain the project ID, log in to the cloud, move the mouse on the account, and click **My Credentials** from the shortcut menu. |
      |                                   |                                                                                                                                                                                                                                                             |
      |                                   | -  Other configuration items are listed after **?** in the form of **key=value**. The configuration items are separated by semicolons (**;**). They can also be passed using the Info object.                                                               |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Info                              | The Info object passes user-defined configuration items. If Info does not pass any attribute item, you can set it to null. The format is as follows: info.setProperty ("Attribute item", "Attribute value").                                                |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _dli_09_0127__en-us_topic_0093946957_table31567601152339:

   .. table:: **Table 3** Attribute items

      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | Item                       | Mandatory                                                                        | Default Value   | Description                                                                                                            |
      +============================+==================================================================================+=================+========================================================================================================================+
      | queuename                  | Yes                                                                              | ``-``           | Queue name of DLI.                                                                                                     |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | databasename               | No                                                                               | ``-``           | Name of a database.                                                                                                    |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | authenticationmode         | No                                                                               | token           | Authentication mode. Currently, token- and AK/SK-based authentication modes are supported.                             |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | accesskey                  | Yes                                                                              | ``-``           | AK/SK authentication key. For details about how to obtain the key, see :ref:`Performing Authentication <dli_09_0121>`. |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | secretkey                  | Yes                                                                              | ``-``           | AK/SK authentication key. For details about how to obtain the key, see :ref:`Performing Authentication <dli_09_0121>`. |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | servicename                | This parameter must be configured if **authenticationmode** is set to **aksk**.  | ``-``           | Indicates the service name, that is, **dli**.                                                                          |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | token                      | This parameter must be configured if **authenticationmode** is set to **token**. | ``-``           | Token authentication. For details about the authentication mode, see :ref:`Performing Authentication <dli_09_0121>`.   |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | charset                    | No                                                                               | UTF-8           | JDBC encoding mode.                                                                                                    |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | usehttpproxy               | No                                                                               | false           | Whether to use the access proxy.                                                                                       |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | proxyhost                  | This parameter must be configured if **usehttpproxy** is set to **true**.        | ``-``           | Access proxy host.                                                                                                     |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | proxyport                  | This parameter must be configured if **usehttpproxy** is set to **true**.        | ``-``           | Access proxy port.                                                                                                     |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | dli.sql.checkNoResultQuery | No                                                                               | false           | Whether to allow invoking the executeQuery API to execute statements (for example, DDL) that do not return results.    |
      |                            |                                                                                  |                 |                                                                                                                        |
      |                            |                                                                                  |                 | -  Value **false** indicates that invoking of the executeQuery API is allowed.                                         |
      |                            |                                                                                  |                 | -  Value **true** indicates that invoking of the executeQuery API is not allowed.                                      |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | jobtimeout                 | No                                                                               | 300             | End time of the job submission. Unit: second                                                                           |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | iam.endpoint               | No. By default, the value is automatically combined based on **regionName**.     | ``-``           | ``-``                                                                                                                  |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | obs.endpoint               | No. By default, the value is automatically combined based on **regionName**.     | ``-``           | ``-``                                                                                                                  |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+
      | directfetchthreshold       | No                                                                               | 1000            | Check whether the number of returned results exceeds the threshold based on service requirements.                      |
      |                            |                                                                                  |                 |                                                                                                                        |
      |                            |                                                                                  |                 | The default threshold is **1000**.                                                                                     |
      +----------------------------+----------------------------------------------------------------------------------+-----------------+------------------------------------------------------------------------------------------------------------------------+

#. Create a Statement object, set related parameters, and submit Spark SQL to DLI.

   **Statement statement = conn.createStatement();**

   statement.execute("SET dli.sql.spark.sql.forcePartitionPredicatesOnPartitionedTable.enabled=true");

   **statement.execute("select \* from tb1");**

#. Obtain the result.

   **ResultSet rs = statement.getResultSet();**

#. Display the result.

   .. code-block::

      while (rs.next()) {
      int a = rs.getInt(1);
      int b = rs.getInt(2);
      }

#. Close the connection.

   **conn.close();**

Example
-------

.. note::

   -  Hard-coded or plaintext AK and SK pose significant security risks. To ensure security, encrypt your AK and SK, store them in configuration files or environment variables, and decrypt them when needed.
   -  In this example, the AK and SK stored in the environment variables are used. Specify the environment variables **System.getenv("AK")** and **System.getenv("SK")** in the local environment first.

.. code-block::

   import java.sql.*;
   import java.util.Properties;

   public class DLIJdbcDriverExample {

       public static void main(String[] args) throws ClassNotFoundException, SQLException {
           Connection conn = null;
           try {
               Class.forName("com.dli.jdbc.DliDriver");
               String url = "jdbc:dli://<endpoint>/<projectId>?databasename=db1;queuename=testqueue";
               Properties info = new Properties();
               info.setProperty("authenticationmode", "aksk");
               info.setProperty("regionname", "<real region name>");
               info.setProperty("accesskey", "<System.getenv("AK")>");
               info.setProperty("secretkey", "<System.getenv("SK")>")
               conn = DriverManager.getConnection(url, info);
               Statement statement = conn.createStatement();
               statement.execute("select * from tb1");
               ResultSet rs = statement.getResultSet();
               int line = 0;
               while (rs.next()) {
                   line ++;
                   int a = rs.getInt(1);
                   int b = rs.getInt(2);
                   System.out.println("Line:" + line + ":" + a + "," + b);
               }
               statement.execute("SET dli.sql.spark.sql.forcePartitionPredicatesOnPartitionedTable.enabled=true");
               statement.execute("describe tb1");
               ResultSet rs1 = statement.getResultSet();
               line = 0;
               while (rs1.next()) {
                   line ++;
                   String a = rs1.getString(1);
                   String b = rs1.getString(2);
                   System.out.println("Line:" + line + ":" + a + "," + b);
               }
           } catch (SQLException ex) {
           } finally {
               if (conn != null) {
                   conn.close();
               }
           }
       }
   }

Enabling JDBC Requery
---------------------

If the JDBC requery function is enabled, the system automatically requeries when the query operation fails.

.. note::

   -  To avoid repeated data insertion, non-query statements do not support requery.
   -  This function is available in the JDBC driver package of 1.1.5 or later. To use this function, obtain the latest JDBC driver package.

To enable the requery function, add the attributes listed in :ref:`Table 4 <dli_09_0127__en-us_topic_0093946957_table127918458100>` to the **Info** parameter.

.. _dli_09_0127__en-us_topic_0093946957_table127918458100:

.. table:: **Table 4** Requery parameter description

   +---------------------+-----------+---------------+----------------------------------------------------------------------------------------------------------------+
   | Item                | Mandatory | Default Value | Description                                                                                                    |
   +=====================+===========+===============+================================================================================================================+
   | USE_RETRY_KEY       | Yes       | false         | Whether to enable the requery function. If this parameter is set to **True**, the requery function is enabled. |
   +---------------------+-----------+---------------+----------------------------------------------------------------------------------------------------------------+
   | RETRY_TIMES_KEY     | Yes       | 3000          | Requery interval (milliseconds). Set this parameter to **30000** ms.                                           |
   +---------------------+-----------+---------------+----------------------------------------------------------------------------------------------------------------+
   | RETRY_INTERVALS_KEY | Yes       | 3             | Requery times. Set this parameter to a value from 3 to 5.                                                      |
   +---------------------+-----------+---------------+----------------------------------------------------------------------------------------------------------------+

Set JDBC parameters, enable the requery function, and create a link. The following is an example:

.. code-block::

   import com.xxx.dli.jdbs.utils.ConnectionResource;// Introduce "ConnectionResource". Change the class name as needed.
   import java.sql.*;
   import java.util.Properties;

   public class DLIJdbcDriverExample {

       private static final String X_AUTH_TOKEN_VALUE = "<realtoken>";
       public static void main(String[] args) throws ClassNotFoundException, SQLException {
           Connection conn = null;
           try {
               Class.forName("com.dli.jdbc.DliDriver");
               String url = "jdbc:dli://<endpoint>/<projectId>?databasename=db1;queuename=testqueue";
               Properties info = new Properties();
               info.setProperty("token", X_AUTH_TOKEN_VALUE);
   info.setProperty(ConnectionResource.USE_RETRY_KEY, "true"); // Enable the requery function.
   info.setProperty(ConnectionResource.RETRY_TIMES_KEY, "30000");// Requery interval (ms)
   info.setProperty(ConnectionResource.RETRY_INTERVALS_KEY, "5");// Requery Times
               conn = DriverManager.getConnection(url, info);
               Statement statement = conn.createStatement();
               statement.execute("select * from tb1");
               ResultSet rs = statement.getResultSet();
               int line = 0;
               while (rs.next()) {
                   line ++;
                   int a = rs.getInt(1);
                   int b = rs.getInt(2);
                   System.out.println("Line:" + line + ":" + a + "," + b);
               }
               statement.execute("describe tb1");
               ResultSet rs1 = statement.getResultSet();
               line = 0;
               while (rs1.next()) {
                   line ++;
                   String a = rs1.getString(1);
                   String b = rs1.getString(2);
                   System.out.println("Line:" + line + ":" + a + "," + b);
               }
           } catch (SQLException ex) {
           } finally {
               if (conn != null) {
                   conn.close();
               }
           }
       }
   }
