:original_name: dli_08_15084.html

.. _dli_08_15084:

Parameter Transfer
==================

Scenario
--------

A UDF can be used in many jobs, and some parameter values vary with jobs. To easily modify the parameter values, you can set **pipeline.global-job-parameters** in the **Runtime Configuration** tab on the Flink OpenSource SQL editing page, and then get the parameter values in the UDF code and use the values as you need. You only need to change the parameter values in the runtime configuration tab to pass the new values to the UDF.

Procedure
---------

Use the open(FunctionContext context) method in your UDF to pass parameters through a FunctionContext object. To pass parameters to a job, perform the following steps:

#. Add **pipeline.global-job-parameters** to **Runtime Configuration** on the Flink OpenSource SQL editing page. The format is as follows:

   .. code-block::

      pipeline.global-job-parameters=k1:v1,"k2:v1,v2",k3:"str:ing","k4:str""ing"

   This configuration defines a map as shown in :ref:`Table 1 <dli_08_15084__table1658123913138>`

   .. _dli_08_15084__table1658123913138:

   .. table:: **Table 1** Examples for pipeline.global-job-parameters

      === ========
      Key Value
      === ========
      k1  v1
      k2  v1,v2
      k3  str:ing
      k4  str""ing
      === ========

   .. note::

      -  **FunctionContext#getJobParameter** obtains only the value of **pipeline.global-job-parameters**. You need to add all key-value pairs that will be used in the UDF to **pipeline.global-job-parameters**.
      -  Keys and values are separated by colons (:). All key-values are connected by commas (,).
      -  If the key or value contains commas (,), use double quotation marks (") to enclose key or value, for example, **"v1,v2"**.
      -  If the key or value contains colons (:), use double quotation marks (") to enclose the key or value, for example, **"str:ing"**.
      -  If the key or value contains a double quotation mark("), use another double quotation mark ("") to escape the first one, and use double quotation marks (") to enclose the key or value, for example, **"str""ing"**.

#. In your UDF code, use **FunctionContext#getJobParameter** to obtain the key-value pairs you set. The code example is as follows:

   .. code-block::

      context.getJobParameter("url","jdbc:mysql://xx.xx.xx.xx:3306/table");
      context.getJobParameter("driver","com.mysql.jdbc.Driver");
      context.getJobParameter("user","user");
      context.getJobParameter("password","password");

Code Samples
------------

The following sample UDF uses **pipeline.global-job-parameters** to pass parameters such as **url**, **user**, and **password** required for connecting to the database, obtains the **udf_info** table data, and combines this data with the stream data into JSON output.

.. table:: **Table 2** udf_info

   ===== =======
   key   value
   ===== =======
   class class-4
   ===== =======

SimpleJsonBuild.java

.. code-block::

   package udf;

   import com.fasterxml.jackson.databind.ObjectMapper;

   import org.apache.flink.table.functions.FunctionContext;
   import org.apache.flink.table.functions.ScalarFunction;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;

   import java.io.IOException;
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.util.HashMap;
   import java.util.Map;

   public class SimpleJsonBuild extends ScalarFunction {
      private static final Logger LOG = LoggerFactory.getLogger(SimpleJsonBuild.class);
      String remainedKey;
      String remainedValue;

      private Connection initConnection(Map<String, String> userParasMap) {
          String url = userParasMap.get("url");
          String driver = userParasMap.get("driver");
          String user = userParasMap.get("user");
          String password = userParasMap.get("password");
          Connection conn = null;
          try {
              Class.forName(driver);
              conn = DriverManager.getConnection(url, user, password);
              LOG.info("connect successfully");
         } catch (Exception e) {
              LOG.error(String.valueOf(e));
         }
          return conn;
     }

      @Override
      public void open(FunctionContext context) throws Exception {
          Map<String, String> userParasMap = new HashMap<>();
          Connection connection;
          PreparedStatement pstmt;
          ResultSet rs;

          String url = context.getJobParameter("url","jdbc:mysql://xx.xx.xx.xx:3306/table");
          String driver = context.getJobParameter("driver","com.mysql.jdbc.Driver");
          String user = context.getJobParameter("user","user");
          String password = context.getJobParameter("password","password");

          userParasMap.put("url", url);
          userParasMap.put("driver", driver);
          userParasMap.put("user", user);
          userParasMap.put("password", password);

          connection = initConnection(userParasMap);
          String sql = "select `key`, `value` from udf_info";
          pstmt = connection.prepareStatement(sql);
          rs = pstmt.executeQuery();

          while (rs.next()) {
              remainedKey = rs.getString(1);
              remainedValue = rs.getString(2);
         }
     }

      public String eval(String... params) throws IOException {
          if (params != null && params.length != 0 && params.length % 2 <= 0) {
              HashMap<String, String> hashMap = new HashMap();
              for (int i = 0; i < params.length; i += 2) {
                  hashMap.put(params[i], params[i + 1]);
                  LOG.debug("now the key is " + params[i].toString() + "; now the value is " + params[i + 1].toString());
             }
              hashMap.put(remainedKey, remainedValue);
              ObjectMapper mapper = new ObjectMapper();
              String result = "{}";
              try {
                  result = mapper.writeValueAsString(hashMap);
             } catch (Exception ex) {
                  LOG.error("Get result failed." + ex.getMessage());
             }
              LOG.debug(result);
              return result;
         } else {
              return "{}";
         }
     }

      public static void main(String[] args) throws IOException {
          SimpleJsonBuild  sjb = new SimpleJsonBuild();
          System.out.println(sjb.eval("json1", "json2", "json3", "json4"));
     }
   }

Add **pipeline.global-job-parameters** to **Runtime Configuration** on the Flink OpenSource SQL editing page. The format is as follows:

.. code-block::

   pipeline.global-job-parameters=url:'jdbc:mysql://x.x.x.x:xxxx/test',driver:com.mysql.jdbc.Driver,user:xxx,password:xxx

Flink OpenSource SQL

.. code-block::

   create function SimpleJsonBuild AS 'udf.SimpleJsonBuild';
   create table dataGenSource(user_id string, amount int) with (
    'connector' = 'datagen',
    'rows-per-second' = '1', --Generate a data record per second.
    'fields.user_id.kind' = 'random', --Specify a random generator for the user_id field.
    'fields.user_id.length' = '3' --Limit the length of user_id to 3.
   );
   create table printSink(message STRING) with ('connector' = 'print');
   insert into
   printSink
   SELECT
   SimpleJsonBuild("name", user_id, "age", cast(amount as string))
   from
   dataGenSource;

Output
------

On the Flin Jobs page, locate your job, and click **More** > **FlinkUI** in the **Operation** column. On the displayed page, click **Task Managers** > **Stdout** to view the job output.

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001742720709.gif
