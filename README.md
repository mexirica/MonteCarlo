# MonteCarlo
Simulação de Monte Carlo em Python

import numpy as np
import pandas as pd
from pandas_datareader import data as wb
import matplotlib.pyplot as plt
from scipy.stats import norm
%matplotlib inline

ticker = 'BBDC4.SA'
data = pd.DataFrame()
data[ticker] = wb.DataReader(ticker, data_source='yahoo', start='2010-1-1')['Adj Close']

log_returns = np.log(1 + data.pct_change())
u = log_returns.mean()
var = log_returns.var()
drift = u - (0.5 * var)
stdev = log_returns.std()

drift.values
stdev.values

t_intervals = 250
iterations = 10

daily_returns = np.exp(drift.values + stdev.values * norm.ppf(np.random.rand(t_intervals, iterations)))

S0 = data.iloc[-1]
S0

price_list = np.zeros_like(daily_returns)
price_list

price_list[0]

price_list[0] = S0
price_list

for t in range(1, t_intervals):
price_list[t] = price_list[t - 1] * daily_returns[t]

price_list

plt.figure(figsize=(10,6))
plt.xlabel('Dias')
plt.ylabel('R$')
plt.title(ticker)
plt.plot(price_list);

![image](https://user-images.githubusercontent.com/67772460/198576752-145c8928-87bc-4881-8757-cdcce4fc5941.png)

![image](https://user-images.githubusercontent.com/67772460/198576776-03eb1a97-ec66-4550-9e28-a90c7590c2d7.png)

![image](https://user-images.githubusercontent.com/67772460/198576809-cf682017-65d6-4450-84c4-68d72101c3a6.png)
