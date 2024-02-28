:original_name: dli_08_0111.html

.. _dli_08_0111:

Time Series Forecasting
=======================

Modeling and forecasting time series is a common task in many business verticals. Modeling is used to extract meaningful statistics and other characteristics of the data. Forecasting is the use of a model to predict future data. DLI provides a series of stochastic linear models to help users conduct online modeling and forecasting in real time.

ARIMA (Non-Seasonal)
--------------------

Auto-Regressive Integrated Moving Average (ARIMA) is a classical model used for time series forecasting and is closely correlated with the AR, MA, and ARMA models.

-  The AR, MA, and ARMA models are applicable to **stationary** sequences.

   -  AR(p) is an autoregressive model. An AR(p) is a linear combination of p consecutive values from immediate past. The model can predict the next value by using the weight of linear combination.
   -  MA(q) is a moving average model. An MA(q) is a linear combination of q white noise values from the past plus the average value. The model can also predict the next value by using the weight of linear combination.
   -  ARMA(p, q) is an autoregressive moving average model, which integrates the advantages of both AR and MA models. In the ARMA model, the autoregressive process is responsible for quantizing the relationship between the current data and the previous data, and the moving average process is responsible for solving problems of random variables. Therefore, the ARMA model is more effective than AR/MA.

-  ARIMA is suitable for **non-stationary** series. In ARIMA(p, q, d), **p** indicates the autoregressive order, **q** indicates the moving average order, and **d** indicates the difference order.

**Syntax**

::

   AR_PRED(field, degree): Use the AR model to forecast new data.
   AR_COEF(field, degree): Return the weight of the AR model.
   ARMA_PRED(field, degree): Use the ARMA model to forecast new data.
   ARMA_COEF(field, degree): Return the weight of the ARMA model.
   ARIMA_PRED(field, degree, derivativeOrder): Use ARIMA to forecast new data.

.. table:: **Table 1** Parameters

   +-----------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | Parameter       | Mandatory | Description                                                                                                                         | Default Value |
   +=================+===========+=====================================================================================================================================+===============+
   | field           | Yes       | Name of the field, data in which is used for prediction, in the data stream.                                                        | ``-``         |
   +-----------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | degree          | No        | Defines how many steps in the past are going to be considered for the next prediction. Currently, only "p = q = degree" is allowed. | 5             |
   +-----------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | derivativeOrder | No        | Derivative order. Generally, this parameter is set to **1** or **2**.                                                               | 1             |
   +-----------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------+---------------+

**Example**

Separately use AR, ARMA, and ARIMA to forecast the time series ordered by rowtime.

::

   SELECT b,
       AR_PRED(b) OVER (ORDER BY rowtime ROWS  BETWEEN 5 PRECEDING AND CURRENT ROW) AS ar,
       ARMA_PRED(b) OVER (ORDER BY rowtime ROWS  BETWEEN 5 PRECEDING AND CURRENT ROW) AS arma,
       ARIMA_PRED(b) OVER (ORDER BY rowtime ROWS  BETWEEN 5 PRECEDING AND CURRENT ROW) AS arima
   FROM MyTable

Holt Winters
------------

The Holt-Winters algorithm is one of the Exponential smoothing methods used to forecast **seasonal** data in time series.

**Syntax**

::

   HOLT_WINTERS(field, seasonality, forecastOrder)

.. table:: **Table 2** Parameters

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                    |
   +=======================+=======================+================================================================================================================================================================================+
   | field                 | Yes                   | Name of the field, data in which is used for prediction, in the data stream.                                                                                                   |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | seasonality           | Yes                   | Seasonality space used to perform the prediction. For example, if data samples are collected daily, and the season space to consider is a week, then **seasonality** is **7**. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | forecastOrder         | No                    | Value to be forecast, specifically, the number of steps to be considered in the future for producing the forecast.                                                             |
   |                       |                       |                                                                                                                                                                                |
   |                       |                       | If **forecastOrder** is set to **1**, the algorithm forecasts the next value.                                                                                                  |
   |                       |                       |                                                                                                                                                                                |
   |                       |                       | If **forecastOrder** is set to **2**, the algorithm forecasts the value of 2 steps ahead in the future. The default value is **1**.                                            |
   |                       |                       |                                                                                                                                                                                |
   |                       |                       | When using this parameter, ensure that the OVER window size is greater than the value of this parameter.                                                                       |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Example**

Use Holt-Winters to forecast time series ordered by rowtime.

::

   SELECT b,
       HOLT_WINTERS(b, 5) OVER (ORDER BY rowtime ROWS  BETWEEN 5 PRECEDING AND CURRENT ROW) AS a1,
       HOLT_WINTERS(b, 5, 2) OVER (ORDER BY rowtime ROWS  BETWEEN 5 PRECEDING AND CURRENT ROW) AS a2
   FROM MyTable
