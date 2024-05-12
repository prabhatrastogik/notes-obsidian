## What is time series?
It is the collection of data points as a sequence over time. Some common types of time series are:
1. **Economic / Financial Time Series** - eg GDP by years; Revenue for each quarter etc.
2. **Marketing Time Series** - Sales MoM
3. **Physical Time Series** - Control variable in manufacturing etc.
4. **Demographic Time Series** - Births by year, population by year etc.

## Types of time series analyses
### Descriptive Analysis
Usually done through a "time plot". Helps to visually understand if there is trend, seasonality or outliers in the series.

### Explanation
Finding the relation between 2 or multiple time series. Common approaches
- Regression Analysis
- Linear Systems (transfer function models - covered later)

### Prediction / Forecasting
Figuring out the next few elements of a time series. Eg - forecasting GDP of next few quarters. (ARIMA models etc.)

### Control Systems
- Simple approaches may involve control charts to ensure tolerances of a process variable are within range
- Complex approaches may involve forecasting future values and if these values are too far from target, then change input parameters to counteract it.

## Transformations of time series
Why are time series transformed?
- **To stabilise variance** - If variance depends on the trend - then transformations like log transforms can help make the variance stable / stationary / independent of trend; If variance changes with time unrelated to trend mean - no transformation will help in stabilising it.
- **To make seasonality additive** - In case amplitude of seasonality depends on trend (mean), then log transforms can help change seasonality from being a multiplicative term to additive term (That can be removed later)

### Box-Cox Transformations (General Class that covers log and sqrt type of transformations)
$$y_t = (x_t^\lambda - 1) / \lambda ~~~~~ \forall ~~~~ \lambda !~= 0$$
$$y_t = \log x_t ~~~~ \forall ~~~~ \lambda = 0$$
Usually Box-Cox parameter **$\lambda$** is estimated so the resultant series $y_t$ is normalized or components like seasonality are additive. BUt - usully these transformations lead to worst outcomes.

However, log transformations can be great in cases % change is important (Log transforms make continuous rates linearly additive across time periods)

## Analysing Trends
### Why analyse trends?
- Trend analysis itself may be the objective. Eg - rate of temperature increase over years due to green house effect
- To remove the trend so that the time series can be made stationary for further analysis. 

### How?
- Curve Fitting:
	- Linear / Quadratic regression w.r.t time. Eg.$$ y_t = \alpha + \beta*t + \epsilon $$
	- Converging S shaped curves like
		- Gompertz Curve: $$ y_t = \alpha*\exp{(\beta*\exp(rt))}$$
		- Logistic Curve: $$ y_t = \frac{a}{(1 + b*\exp(-ct))}$$
- Filtering
	- Linear filter with $\sum weights$ = 1 $$y_t = \sum _{r=-q} ^s a_rx_{t+r}~~~\land ~~~\sum a_r = 1$$
	- Moving Average is special linear filter where $s = q$ and $a_r = \frac{1}{2q+1}$ (symmetric smoothing)
		- Symmetric smoothing has a **"end-effects"** problem {basically it can be only computed from q+1 to N-q positions, most forecasting methods need smoothing till last element in the series}
	- Exponential Smoothing $$y_t = \sum _{t = 0} ^\infty \alpha*(1=\alpha)^j*x_{t-j})$$