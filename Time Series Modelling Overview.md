## What is a Time Series?

A time series is a collection of data points recorded in chronological order, capturing how certain variables change over time. Common types of time series include:

1. **Economic/Financial Time Series**: Examples include GDP measured annually or quarterly revenue figures.
2. **Marketing Time Series**: Examples include monthly sales figures.
3. **Physical Time Series**: Examples include control variables in manufacturing processes.
4. **Demographic Time Series**: Examples include annual birth rates or population statistics.

## Types of Time Series Analyses

### Descriptive Analysis

This involves visualizing the data using a "time plot" to identify trends, seasonality, and outliers. It helps in understanding the overall behavior of the time series.

### Explanation

This involves finding relationships between two or more time series. Common approaches include:

- **Regression Analysis**: To understand how one time series affects another.
- **Linear Systems (Transfer Function Models)**: To model the relationship between inputs and outputs in a system, which will be discussed later.

### Prediction / Forecasting

This involves predicting future values of a time series. For example, forecasting GDP for the next few quarters using models like ARIMA.

### Control Systems

- **Simple Approaches**: Use control charts to ensure that process variables stay within specified tolerances.
- **Complex Approaches**: Involve forecasting future values and adjusting input parameters if forecasts deviate significantly from targets.

## Transformations of Time Series

### Why Transform Time Series?

- **To Stabilize Variance**: If variance depends on the trend, transformations like log transforms can help make the variance stable or stationary, independent of the trend. If variance changes over time without a clear trend, transformations may not help.
- **To Make Seasonality Additive**: If the amplitude of seasonality depends on the trend, log transforms can convert multiplicative seasonality into additive seasonality, which can then be removed.

### Box-Cox Transformations

Box-Cox transformations are a general class of transformations that include log and square root transformations. They are defined as:

$$ y_t = \frac{x_t^\lambda - 1}{\lambda} \quad \forall \lambda \neq 0 $$
$$ y_t = \log x_t \quad \forall \lambda = 0 $$

The Box-Cox parameter \(\lambda\) is usually estimated to normalize the series or make components like seasonality additive. However, these transformations can sometimes lead to worse outcomes. Log transformations can be particularly useful when percentage changes are important, as they make continuous rates linearly additive across time periods.

## Analyzing Trends

### Why Analyze Trends?

- **To Understand Long-Term Changes**: For example, analyzing the rate of temperature increase over the years due to greenhouse effects.
- **To Remove Trends for Stationarity**: Removing trends can make the time series stationary, which is necessary for certain types of analysis.

### How to Analyze Trends

#### Curve Fitting

- **Linear/Quadratic Regression**: Fit a linear or quadratic model to the time series. For example:

  $$ y_t = \alpha + \beta t + \epsilon $$

- **Nonlinear Models**: Fit converging S-shaped curves like the Gompertz curve or the logistic curve:

  - Gompertz Curve: $$ y_t = \alpha \exp(\beta \exp(rt)) $$
  - Logistic Curve: $$ y_t = \frac{a}{1 + b \exp(-ct)} $$

#### Filtering

- **Linear Filters**: Apply a linear filter with weights that sum to 1:

  $$ y_t = \sum_{r=-q}^{s} a_r x_{t+r} \quad \text{where} \quad \sum a_r = 1 $$

- **Moving Average**: A special linear filter where \(s = q\) and \(a_r = \frac{1}{2q+1}\) (symmetric smoothing). Symmetric smoothing has "end-effects" problems as it can only be computed from \(q+1\) to \(N-q\) positions, but most forecasting methods need smoothing until the last element in the series.

- **Exponential Smoothing**:

  $$ y_t = \sum_{j=0}^{\infty} \alpha (1 - \alpha)^j x_{t-j} $$
