:original_name: dli_08_15082.html

.. _dli_08_15082:

UDFs
====

Overview
--------

DLI supports the following three types of user-defined functions (UDFs):

-  Regular UDF: takes in one or more input parameters and returns a single result.
-  User-defined table-generating function (UDTF): takes in one or more input parameters and returns multiple rows or columns.
-  User-defined aggregate function (UDAF): aggregates multiple records into one value.

.. note::

   Currently, UDF, UDTF, or UDAF custom functions cannot be written using Python.

POM Dependency
--------------

.. code-block::

   <dependency>
       <groupId>org.apache.flink</groupId>
       <artifactId>flink-table-common</artifactId>
       <version>1.15.0</version>
       <scope>provided</scope>
   </dependency>

Using UDFs
----------

#. Encapsulate the implemented UDFs into a JAR package and upload the package to OBS.
#. In the navigation pane of the DLI management console, choose **Data Management** > **Package Management**. On the displayed page, click **Create** and use the JAR package uploaded to OBS to create a package.
#. In the left navigation, choose **Job Management** and click **Flink Jobs**. Locate the row where the target resides and click **Edit** in the **Operation** column to switch to the page where you can edit the job.
#. Click the **Running Parameters** tab of your job, select the UDF JAR and click **Save**.
#. Add the following statement to the SQL statements to use the functions:

UDF
---

The regular UDF must inherit the ScalarFunction function and implement the eval method. The open and close functions are optional.

**Example code**

.. code-block::

   import org.apache.flink.table.functions.FunctionContext;
   import org.apache.flink.table.functions.ScalarFunction;

   public class UdfScalarFunction extends ScalarFunction {
       private int factor = 12;
       public UdfScalarFunction() {
           this.factor = 12;
       }
       /**
      * (optional) Initialization
        * @param context
        */
       @Override
       public void open(FunctionContext context) {}
       /**
      * Custom logic
        * @param s
        * @return
        */
       public int eval(String s) {
           return s.hashCode() * factor;
       }
       /**
       * Optional
        */
       @Override   public void close() {}
   }

**Example**

::

   CREATE FUNCTION udf_test AS 'com.company.udf.UdfScalarFunction';
   INSERT INTO sink_stream select udf_test(attr) FROM source_stream;

UDTF
----

The UDTF must inherit the TableFunction function and implement the eval method. The open and close functions are optional. If the UDTF needs to return multiple columns, you only need to declare the returned value as **Tuple** or **Row**. If **Row** is used, you need to overload the getResultType method to declare the returned field type.

**Example code**

.. code-block::

   import org.apache.flink.api.common.typeinfo.TypeInformation;
   import org.apache.flink.api.common.typeinfo.Types;
   import org.apache.flink.table.functions.FunctionContext;
   import org.apache.flink.table.functions.TableFunction;
   import org.apache.flink.types.Row;

   public class UdfTableFunction extends TableFunction<Row> {
       /**
      * (optional) Initialization
        * @param context
        */
       @Override
       public void open(FunctionContext context) {}
       public void eval(String str, String split) {
           for (String s : str.split(split)) {
               Row row = new Row(2);
               row.setField(0, s);
               row.setField(1, s.length());
               collect(row);
           }
       }
       /**
      * Declare the type returned by the function
        * @return
        */
       @Override
       public TypeInformation<Row> getResultType() {
           return Types.ROW(Types.STRING, Types.INT);
       }
       /**
       * Optional
        */
       @Override
       public void close() {}
   }

**Example**

The UDTF supports CROSS JOIN and LEFT JOIN. When the UDTF is used, the **LATERAL** and **TABLE** keywords must be included.

-  CROSS JOIN: does not output the data of a row in the left table if the UDTF does not output the result for the data of the row.
-  LEFT JOIN: outputs the data of a row in the left table even if the UDTF does not output the result for the data of the row, but pads null with UDTF-related fields.

::

   CREATE FUNCTION udtf_test AS 'com.company.udf.TableFunction';
   // CROSS JOIN
   INSERT INTO sink_stream select subValue, length FROM source_stream, LATERAL
   TABLE(udtf_test(attr, ',')) as T(subValue, length);
   // LEFT JOIN
   INSERT INTO sink_stream select subValue, length FROM source_stream LEFT JOIN LATERAL
   TABLE(udtf_test(attr, ',')) as T(subValue, length) ON TRUE;

UDAF
----

The UDAF must inherit the AggregateFunction function. You need to create an accumulator for storing the computing result, for example, **WeightedAvgAccum** in the following example code.

**Example code**

.. code-block::

   public class WeightedAvgAccum {
   public long sum = 0;
   public int count = 0;
   }

.. code-block::

   import org.apache.flink.table.functions.AggregateFunction;

   import java.util.Iterator;

   /**
   * The first type variable is the type returned by the aggregation function, and the second type variable is of the Accumulator type.
    * Weighted Average user-defined aggregate function.
    */
   public class UdfAggFunction extends AggregateFunction<Long, WeightedAvgAccum> {
   // Initialize the accumulator.
       @Override
       public WeightedAvgAccum createAccumulator() {
           return new WeightedAvgAccum();
       }
   // Return the intermediate computing value stored in the accumulator.
       @Override
       public Long getValue(WeightedAvgAccum acc) {
           if (acc.count == 0) {
               return null;
           } else {
               return acc.sum / acc.count;
           }
       }
   // Update the intermediate computing value according to the input.
       public void accumulate(WeightedAvgAccum acc, long iValue) {
           acc.sum += iValue;
           acc.count += 1;
       }
   // Perform the retraction operation, which is opposite to the accumulate operation.
       public void retract(WeightedAvgAccum acc, long iValue) {
           acc.sum -= iValue;
           acc.count -= 1;
       }
   // Combine multiple accumulator values.
       public void merge(WeightedAvgAccum acc, Iterable<WeightedAvgAccum> it) {
           Iterator<WeightedAvgAccum> iter = it.iterator();
           while (iter.hasNext()) {
               WeightedAvgAccum a = iter.next();
               acc.count += a.count;
               acc.sum += a.sum;
           }
       }
   // Reset the intermediate computing value.
       public void resetAccumulator(WeightedAvgAccum acc) {
           acc.count = 0;
           acc.sum = 0L;
       }
   }

**Example**

::

   CREATE FUNCTION udaf_test AS 'com.company.udf.UdfAggFunction';
   INSERT INTO sink_stream SELECT udaf_test(attr2) FROM source_stream GROUP BY attr1;
