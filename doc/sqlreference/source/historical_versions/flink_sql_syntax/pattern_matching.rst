:original_name: dli_08_0108.html

.. _dli_08_0108:

Pattern Matching
================

Complex event processing (CEP) is used to detect complex patterns in endless data streams so as to identify and search patterns in various data rows. Pattern matching is a powerful aid to complex event handling.

CEP is used in a collection of event-driven business processes, such as abnormal behavior detection in secure applications and the pattern of searching for prices, transaction volume, and other behavior in financial applications. It also applies to fraud detection and sensor data analysis.

Syntax
------

::

   MATCH_RECOGNIZE (
         [ PARTITION BY expression [, expression ]* ]
         [ ORDER BY orderItem [, orderItem ]* ]
         [ MEASURES measureColumn [, measureColumn ]* ]
         [ ONE ROW PER MATCH | ALL ROWS PER MATCH ]
         [ AFTER MATCH
               ( SKIP TO NEXT ROW
               | SKIP PAST LAST ROW
               | SKIP TO FIRST variable
               | SKIP TO LAST variable
               | SKIP TO variable )
         ]
         PATTERN ( pattern )
         [ WITHIN intervalLiteral ]
         DEFINE variable AS condition [, variable AS condition ]*
   ) MR

.. note::

   Pattern matching in SQL is performed using the MATCH_RECOGNIZE clause. MATCH_RECOGNIZE enables you to do the following tasks:

   -  Logically partition and order the data that is used in the MATCH_RECOGNIZE clause with its PARTITION BY and ORDER BY clauses.
   -  Define patterns of rows to seek using the PATTERN clause of the MATCH_RECOGNIZE clause. These patterns use regular expression syntax.
   -  Specify the logical conditions required to map a row to a row pattern variable in the DEFINE clause.
   -  Define measures, which are expressions usable in other parts of the SQL query, in the MEASURES clause.

Syntax description
------------------

.. table:: **Table 1** Syntax description

   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                       | Mandatory             | Description                                                                                                                                     |
   +=================================+=======================+=================================================================================================================================================+
   | PARTITION BY                    | No                    | Logically divides the rows into groups.                                                                                                         |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | ORDER BY                        | No                    | Logically orders the rows in a partition.                                                                                                       |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | [ONE ROW \| ALL ROWS] PER MATCH | No                    | Chooses summaries or details for each match.                                                                                                    |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | -  ONE ROW PER MATCH: Each match produces one summary row.                                                                                      |
   |                                 |                       | -  ALL ROWS PER MATCH: A match spanning multiple rows will produce one output row for each row in the match.                                    |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | The following provides an example:                                                                                                              |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | .. code-block::                                                                                                                                 |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       |          SELECT * FROM MyTable MATCH_RECOGNIZE                                                                                                  |
   |                                 |                       |            (                                                                                                                                    |
   |                                 |                       |              MEASURES AVG(B.id) as Bid                                                                                                          |
   |                                 |                       |              ALL ROWS PER MATCH                                                                                                                 |
   |                                 |                       |              PATTERN (A B C)                                                                                                                    |
   |                                 |                       |              DEFINE                                                                                                                             |
   |                                 |                       |                A AS A.name = 'a',                                                                                                               |
   |                                 |                       |                B AS B.name = 'b',                                                                                                               |
   |                                 |                       |                C as C.name = 'c'                                                                                                                |
   |                                 |                       |            ) MR                                                                                                                                 |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | Example description                                                                                                                             |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | Assume that the format of MyTable is (id, name) and there are three data records: (1, a), (2, b), and (3, c).                                   |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | ONE ROW PER MATCH outputs the average value 2 of B.                                                                                             |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | ALL ROWS PER MATCH outputs each record and the average value of B, specifically, (1,a, null), (2,b,2), (3,c,2).                                 |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | MEASURES                        | No                    | Defines calculations for export from the pattern matching.                                                                                      |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | PATTERN                         | Yes                   | Defines the row pattern that will be matched.                                                                                                   |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | -  PATTERN (A B C) indicates to detect concatenated events A, B, and C.                                                                         |
   |                                 |                       | -  PATTERN (A \| B) indicates to detect A or B.                                                                                                 |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | -  Modifiers                                                                                                                                    |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       |    -  \*: 0 or more iterations. For example, A\* indicates to match A for 0 or more times.                                                      |
   |                                 |                       |    -  +: 1 or more iterations. For example, A+ indicates to match A for 1 or more times.                                                        |
   |                                 |                       |    -  ? : 0 or 1 iteration. For example, A? indicates to match A for 0 times or once.                                                           |
   |                                 |                       |    -  {n}: *n* iterations (*n* > 0). For example, A{5} indicates to match A for five times.                                                     |
   |                                 |                       |    -  {n,}: *n* or more iterations (*n* >= 0). For example, A{5,} indicates to match A for five or more times.                                  |
   |                                 |                       |    -  {n, m}: between *n* and *m* (inclusive) iterations (0 <= *n* <= *m*, 0 < *m*). For example, A{3,6} indicates to match A for 3 to 6 times. |
   |                                 |                       |    -  {, m}: between 0 and *m* (inclusive) iterations (*m* > 0). For example, A{,4} indicates to match A for 0 to 4 times.                      |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | DEFINE                          | Yes                   | Defines primary pattern variables.                                                                                                              |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | AFTER MATCH SKIP                | No                    | Defines where to restart the matching process after a match is found.                                                                           |
   |                                 |                       |                                                                                                                                                 |
   |                                 |                       | -  SKIP TO NEXT ROW: Resumes pattern matching at the row after the first row of the current match.                                              |
   |                                 |                       | -  SKIP PAST LAST ROW: Resumes pattern matching at the next row after the last row of the current match.                                        |
   |                                 |                       | -  SKIP TO FIRST variable: Resumes pattern matching at the first row that is mapped to the pattern variable.                                    |
   |                                 |                       | -  SKIP TO LAST variable: Resumes pattern matching at the last row that is mapped to the pattern variable.                                      |
   |                                 |                       | -  SKIP TO variable: Same as SKIP TO LAST variable.                                                                                             |
   +---------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+

Functions Supported by CEP
--------------------------

.. table:: **Table 2** Function description

   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function                                        | Description                                                                                                                                                                                                                                                                                                                                                                              |
   +=================================================+==========================================================================================================================================================================================================================================================================================================================================================================================+
   | MATCH_NUMBER()                                  | Finds which rows are in which match. It can be used in the MEASURES and DEFINE clauses.                                                                                                                                                                                                                                                                                                  |
   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CLASSIFIER()                                    | Finds which pattern variable applies to which rows. It can be used in the MEASURES and DEFINE clauses.                                                                                                                                                                                                                                                                                   |
   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FIRST()/LAST()                                  | FIRST returns the value of an expression evaluated in the first row of the group of rows mapped to a pattern variable. LAST returns the value of an expression evaluated in the last row of the group of rows mapped to a pattern variable. In PATTERN (A B+ C), FIRST (B.id) indicates the ID of the first B in the match, and LAST (B.id) indicates the ID of the last B in the match. |
   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | NEXT()/PREV()                                   | Relative offset, which can be used in DEFINE. For example, PATTERN (A B+) DEFINE B AS B.price > PREV(B.price)                                                                                                                                                                                                                                                                            |
   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RUNNING/FINAL                                   | RUNNING indicates to match the middle value, while FINAL indicates to match the final result value. Generally, RUNNING/FINAL is valid only in ALL ROWS PER MATCH. For example, if there are three records (a, 2), (b, 6), and (c, 12), then the values of RUNNING AVG (A.price) and FINAL AVG (A.price) are (2,6), (4,6), (6,6).                                                         |
   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Aggregate functions (COUNT, SUM, AVG, MAX, MIN) | Aggregation operations. These functions can be used in the MEASURES and DEFINE clauses. For details, see :ref:`Aggregate Functions <dli_08_0104>`.                                                                                                                                                                                                                                       |
   +-------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  Fake plate vehicle detection

CEP conducts pattern matching based on license plate switchover features on the data of vehicles collected by cameras installed on urban roads or high-speed roads in different areas within 5 minutes.

::

   INSERT INTO fake_licensed_car
   SELECT * FROM camera_license_data MATCH_RECOGNIZE
   (
     PARTITION BY car_license_number
     ORDER BY proctime
     MEASURES A.car_license_number as car_license_number, A.camera_zone_number as first_zone, B.camera_zone_number as second_zone
     ONE ROW PER MATCH
     AFTER MATCH SKIP TO LAST C
     PATTERN (A B+ C)
     WITHIN interval '5' minute
     DEFINE
       B AS B.camera_zone_number <> A.camera_zone_number,
       C AS C.camera_zone_number = A.camera_zone_number
   ) MR;

According to this rule, if a vehicle of a license plate number drives from area A to area B but another vehicle of the same license plate number is detected in area A within 5 minutes, then the vehicle in area A is considered to carry a fake license plate.

Input data:

.. code-block::

   Zhejiang B88888, zone_A
   Zhejiang AZ626M, zone_A
   Zhejiang B88888, zone_A
   Zhejiang AZ626M, zone_A
   Zhejiang AZ626M, zone_A
   Zhejiang B88888, zone_B
   Zhejiang B88888, zone_B
   Zhejiang AZ626M, zone_B
   Zhejiang AZ626M, zone_B
   Zhejiang AZ626M, zone_C
   Zhejiang B88888, zone_A
   Zhejiang B88888, zone_A

The output is as follows:

.. code-block::

   Zhejiang B88888, zone_A, zone_B
