# Studying Bitcoin's Historical Volatility

Bitcoin's volatility is a source of continues debates because a high volatility impairs its use as medium of exchange and unit of account in economic exchanges: it therefore prevents Bitcoin to be adopted as money. 

During Bitcoin's early days, Bitcoin's supporters advanced the hypothesis that its future volatility would be inversely correlated to the size of its market capitalization, i.e the outstanding value of all Bitcoins. 

In early 2021, Bitcoin's market capitalization reached 1 trillion dollars, putting it on pars with major fiat currencies such as the Swiss franc and a considerable fraction of the investable gold market. But is Bitcoin's volatility lower today than when its marketcap was hundreds of time smaller? This is the focus on this investigation.

## Volatility

A tradable asset can be described by a discrete time series of prices $S_{n}$. This time series is a sample, taken with a constant sampling frequency that can be daily, hourly or even at the minute level, of the entire tick-by-tick, trade-by-trade price history of the asset, that for we can consider continous. That price history is the population from with the extract our sample: the aim of our stastitical analysis is inferring properties of the population by analyzing data from the sample, and we will see how. 

Volatility is generally defined as a measure of dispersion of the price returns in the time series. Since we are analyzing data fromn a sample taken from the population, we can only estimate volatility using statistical estimators. Ideally, we want a statistical estimator that it is unbiased and efficient. 
In statistics, bias is defined as the difference between an estimator expected value and the true value of the parameter being estimated. 
We also want an estimator that is stable from sample to sample: the less prone to sampling error an estimator is, the more efficient it is.

The most common statistical estimator used for volatility is the square rooot of the variance of the price returns in the sample, which also it is the definition for the standard deviation of a normal distribution. Volatility is usually reported as an annualized metrics, so that the resulting variance has to be multiplied by the number of trading periods in one year.  

Let's assume that we sample a time series of N consecutive prices $S_{n}$ of our security. Then the sample variance is defined as
$$
\begin{align*}
s^2 = \frac{1}{N} \sum_{i=1}^N \left(x_i - \bar x \right)^2
\end{align*}
$$

where $ x_i = \ln\left(\frac{S_i}{S_{i-1}}\right) $  and $\bar x = \frac{1}{N} \sum_{i=1}^N x_i $ is the mean log returns in the sample, also called drift. As the drift, which is a log return, may greatly depend on the sample considered and it is should be small in the period considered, by setting $\bar x = 0$ we reduce a source of potential noise in our volatility estimate. 

Since we are actually interested in the population variance, we must know how to infer it from the sample variance. The sample variance defined above is a well known *biased estimator* of the population variance $\sigma^2$, so that it needs to be multiplied by a factor $n/(n-1)$ to remove the bias {doc}`statistics.ipynb` (this is called Bernoulli's correction):
$$
\begin{align*}
\sigma^2 = \frac{N}{N - 1} s^2
\end{align*}
$$


We can already see that the sample size N is quite important, but it becomes even more important if we consider the variance of the sample variance itself:
$$
\begin{align*}
var(s) \approx \frac{\sigma^2}{2N} 
\end{align*}

In fact, for small $N$ such as the commonly used $N=20$ or $N=30$, the uncertainity on the sample variance can be quite high. Much better to use N of 60 and above.

So why shouldn't we use always a high N in our volatility estimate? Firstly, a high N could make jumps or trading discountinity persist in our estimate, leading to an overestimate. Secondly, since often we use daily closes as pricing data, looking up to 60 days in the past may provide us with information on volatility which are not anymore current. 

**Square Root**