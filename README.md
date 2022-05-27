# Observing Ether Value in Basic Econometrics Method
> 使用基本計量方法觀察以太幣價格因素

**Article Quick Look**
* [Run this Demo in Google Colab](https://colab.research.google.com/drive/18uAPyIbSiSVVl8U8zZaej0_6lt6P5CQ6?usp=sharing)
* [See the Slide in Google Presentation](https://docs.google.com/presentation/d/1CKOaILPyzXpkKC7QmNm82kAXyJszobXTEzcy0KmEnTs/edit?usp=sharing)

## 目錄

* **研究介紹**
    * 動機與目的、研究流程
* **資料描述**
    * 資料類型與 EDA
* **資料處理、特徵工程**
    * 資料預處理、應變數、自變數介紹
* **模型**
    * 資料檢測
    * 模型分析
    * 模型介紹與結果比較
* **總結**
    * 應用、結論

---

## 研究介紹

### 研究動機

* 貨幣政策
    * 研究以太坊上的貨幣政策是否對幣價有直接影響，包含供給量、手續費、燃燒量等
* 安全性
    * 研究以太坊上的安全性變數是否對幣價有直接影響，包含哈希率、區塊難度、礦工節點數、礦工收入等
* 擴容性
    * 研究以太坊上的金融和去中心化服務是否對幣價有直接影響，包含鎖倉量、NFT 銷售量、穩定幣市值等


### 研究目的

使用迴歸分析找到有說服力的重要變數，可藉此預測幣價和應用於選擇權的去中心化應用程式程式

### 研究流程

![](https://i.imgur.com/3cawGve.png)

---

## 資料描述

[**DataSet 2015/7/30~2022/5/12**](https://docs.google.com/spreadsheets/d/1DlyoV5HBEbYAJz0tH8Ul8yE3WRAXSHeLuQhdLDgino8/edit?usp=sharing)

### Explanatory & Response Variables

| Explanatory Variables | Statement | 
| -------- | -------- | 
|**1. On-Chain Data**||
|**Monetary Situation**||
| Total Supply | 總發行量 |
|  Market Cap | 以太幣總市值 |
|  Daily Ether Burnt | 每日銷毀量 |
|||
| Average Gas Price | 燃料費 |
|  Average Gas Limit |平均交易最高燃料量
 |
|  Average Transaction Fee | 平均交易手續費 |
|  Gas Used Per Day | 每日 Gas 使用量 |
|||
|  Blocks Per Day | 每日出產區塊 |
|  Average Block Size | 平均區塊大小 |
|  Average Difficulty | 平均區塊難度 |
|**Security 安全性**||
|  Network Hash Rate | 可以衡量礦工保護網路免受攻擊的處理能力 |
| Miner Revenue | 新鑄造的以太幣和交易費用的加總，礦工收入的高低也會影響到礦工的打包意願|
| Active Nodes | 運行節點數|
|**Usage**| 觀察網路使用量可以給我們當前經濟活動的相關訊息 |
| Active Adress | 活躍地址數量|
|  Cumulative Unique Addresses | 累積地址數量 |
|  Network Utilization | 網路使用量|
|  Transactions Per Day | 每日交易量 |
|**Capability**| 支援智能合約的運作讓其之上的 Dapp 可以促成許多不同的產品和經濟模式。|
|DeFi TVL| 總鎖倉量 |
| Verified Contracts | 每日驗證合約數量 |
|StableCoins Value| 穩定幣總價值，暫定為 USDT、USDC、DAI 總和 |
|Sales NFT per Day | NFT 每日銷售額 |
|Number of sales NFT per Day| NFT 每日銷售量 |
|||
|**3. Competition & Correlation**|
| NASDAQ | 美股那斯達克指數 |
| BTC/USDT Price | 比特幣價格 |
| SOL/USDT Price | Solana 價格 |
| ADA/USDT Price | Cardano 價格 |
| BNB/USDT Price | 幣安幣價格 |
| ETC/USDT Price | 以太坊經典價格 |
|||
| **Response Variables**| **Statement** |
| ETH/USDT Price | 以太幣價格| 

### Additional Remarks

1. Speed and Scalability and ETH 2.0
    * 以太坊當前出塊速度為 12~14 秒、交易結算時間約為五分鐘
    * ETH 2.0 將會達到每秒 15000 筆交易，然而 Beacon Chain 的影響在我們目前單就以太坊內部分析來講，對交易來說都只是常數，所以之後我們不會納入分析。
2. 穩定幣 Stablecoins：
    * 穩定幣代表加密資產們如何和現實世界錨定的一種規則，或許我們可以使用穩定幣的發行數量來觀察現實世界使用區塊鏈的數量關係，不只如此，這也是使用者最直接衡量加密資產或服務價值的方式。同時穩定幣的使用也和鏈上產品和應用程式息息相關，對穩定幣的需求或許會直接或間接地表明使用者的習慣和需求
3. Internal governance
    * Proof-of-Stake Model 中治理代幣可能會對價格產生影響，但由於尚未實行所以目前尚不考慮這個因素
4. Competition & Correlation
    * 比特幣作為整個加密貨幣市場的主宰，其波動自然也會帶動其他貨幣的發展，雖然數據顯示從 2020 的 DeFi 爆炸性發展之後 ETH 和 BTC 的相關性就不斷下降，但我想競爭幣的價格還是有可以參考的部分
    * 這邊除了 BTC 之外我還選出了其他的主流生態系的加密貨幣來作為解釋變數，看能不能發現一些相關性。
    * 此外科技股 NASDAQ 指數也和加密貨幣市場有相關性的可能，因此也納入考量

:::warning
這裡的分析將不會考慮穩定幣的脫鉤。
:::

---

## 資料處理、特徵工程

### Data Preprocessing

1. 那斯達克指數的資料缺少了六日和某些天數（不知明原因），但我們都知道區塊鏈是沒有在放假的所以這邊簡單用 python 補上，那缺少的六日資料我們沿用當周禮拜五的資料來做填補遺漏值
2. 從 EDA 的 ETH 走勢我們可以清楚看見在 75%（約為 2020/10/9）前後的資料是完全不同模型，所以未來會將其切成兩份進行分析


### TS'2 No Perfect Multicollinearity

![](https://i.imgur.com/1paWR73.png)


1. 我觀察到 Market Cap 和 ETH/USDT 是完全線性關係，所以將其從 X 中剔除
2. 同時 Hash Rate 和 Block Difficulty 也是線性關係，但因為去除其中有可能會導致最後無法解釋，所以暫時留著（未來在 OLS 會自動排除）！
3. 增加四組 ETH_lag 變數，延期一天、一旬、兩旬、一個月。


### EDA（探索式資料分析）

接下來進入 EDA 的部分，這邊分成兩個步驟來寫，一個是 Response Variable，另外一個是 Explanatory。

![](https://i.imgur.com/9qB8r1A.png)

![](https://i.imgur.com/tqHWCIx.png)


比較特別的是 Explanatory Variables 我有做正規化來讓 Ploting 的樣子更好觀察。畢竟觀察絕對距離沒什麼意義，使用相對距離來呈現趨勢和相關性我自己覺得更為合適！

![](https://i.imgur.com/89Syq09.png)

可以觀察到在資料前 75% 的部分競爭幣的趨勢和以太、比特非常類似，但在後 25% 的部分都有走出自己的趨勢。也可以很明顯看出那斯達克指數有獨特的趨勢。

![](https://i.imgur.com/R1iDTHx.png)

可以發現獨特地址、交易數量等變數有一個明顯向上的趨勢。

---

## Model 1 - Original OLS

直接使用原始的資料做最基本沒有任何變化的 OLS：

```
                       Original OLS Regression Results                        
==============================================================================
Dep. Variable:               ETH/USDT   R-squared:                       0.992
Model:                            OLS   Adj. R-squared:                  0.992
Method:                 Least Squares   F-statistic:                     2519.
Date:                Fri, 27 May 2022   Prob (F-statistic):               0.00
Time:                        04:51:27   Log-Likelihood:                -3509.7
No. Observations:                 579   AIC:                             7075.
Df Residuals:                     551   BIC:                             7198.
Df Model:                          27                                         
Covariance Type:            nonrobust                                         
===============================================================================================
                                  coef    std err          t      P>|t|      [0.025      0.975]
-----------------------------------------------------------------------------------------------
const                        1.134e+04   4383.332      2.586      0.010    2727.228    1.99e+04
ETH_lag1                      759.4783     63.545     11.952      0.000     634.658     884.298
ETH_lag10                     -42.0006     42.332     -0.992      0.322    -125.152      41.151
ETH_lag20                      70.7987     41.857      1.691      0.091     -11.420     153.017
ETH_lag30                      23.3791     37.873      0.617      0.537     -51.015      97.773
EtherSupply                   -2.8e-05   1.69e-05     -1.653      0.099   -6.13e-05    5.27e-06
Daily Ether Burnt(ETH)         -0.0294      0.004     -8.057      0.000      -0.037      -0.022
Gas Limit                      -0.0003   8.43e-05     -3.181      0.002      -0.000      -0.000
Gas Price(Wei)              -2.833e-09   3.06e-10     -9.258      0.000   -3.43e-09   -2.23e-09
Gas Used Per Day             4.356e-08   1.41e-08      3.090      0.002    1.59e-08    7.12e-08
Average Txn Fee (USD)          17.6229      1.772      9.945      0.000      14.142      21.104
Blocks Per Day                 -0.4906      0.258     -1.898      0.058      -0.998       0.017
Average Block Size(Bytes)      -0.0071      0.001     -6.829      0.000      -0.009      -0.005
BlockDifficulty                 0.4270      0.113      3.773      0.000       0.205       0.649
Hash Rate(GH/S)                -0.0046      0.001     -3.150      0.002      -0.007      -0.002
Cumulative Unique Addresses -1.387e-05   3.53e-06     -3.928      0.000   -2.08e-05   -6.93e-06
Network Utilization         -8963.8744   2509.510     -3.572      0.000   -1.39e+04   -4034.498
TransactionsCount Per Day       0.0008   9.66e-05      7.907      0.000       0.001       0.001
DeFi_TVL                     1.032e-08    6.8e-10     15.173      0.000    8.99e-09    1.17e-08
Verified Contracts              0.0446      0.080      0.556      0.578      -0.113       0.202
StableCoins Value            -5.77e-09   2.36e-09     -2.446      0.015   -1.04e-08   -1.14e-09
Sales NFT per Day            6.994e-09   1.64e-07      0.043      0.966   -3.16e-07     3.3e-07
Number of sales NFT per Day     0.0004      0.000      1.680      0.093    -6.1e-05       0.001
NASDAQ                          0.0131      0.015      0.854      0.394      -0.017       0.043
BTC/USDT                        0.0024      0.002      1.491      0.137      -0.001       0.005
BNB/USDT                       -0.4237      0.119     -3.551      0.000      -0.658      -0.189
SOL/USDT                       -0.0826      0.333     -0.248      0.804      -0.737       0.571
ADA/USDT                       -0.8266     29.766     -0.028      0.978     -59.295      57.642
ETC/USDT                        4.4632      0.718      6.218      0.000       3.053       5.873
==============================================================================
Omnibus:                       64.486   Durbin-Watson:                   1.403
Prob(Omnibus):                  0.000   Jarque-Bera (JB):              309.225
Skew:                           0.348   Prob(JB):                     7.12e-68
Kurtosis:                       6.512   Cond. No.                     2.44e+14
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 2.44e+14. This might indicate that there are
strong multicollinearity or other numerical problems.
```
### TS'4 Heteroscedasticity
Heteroscedasticity 代表應變數在不同的 level 有不同的散布情形。其中一個驗證方法是透過視覺化的圖查看 OLS model residuals，如果是 homoskedastic 就不會有 residuals pattern。

如果 errors 是 heteroskedastic，變異數可能會有上升或下降的趨勢，例如在比較大的 y_hat 或比較大的 x_j 時變異數會上升。


![](https://i.imgur.com/KlCU87u.png)

### TS'4 Heteroscedasticity-Bruesch-Pagan Test

Bruesch-Pagan Test
* The null hypothesis (H0): Homoscedasticity is present.
* The alternative hypothesis: (Ha): Homoscedasticity is not present (i.e. heteroscedasticity exists)

```python=
from statsmodels.stats.diagnostic import het_white, het_breuschpagan
#perform White's test
breuschpagan_test = het_breuschpagan(model.resid, model.model.exog)

#define labels to use for output of White's test
labels = ['Test Statistic', 'Test Statistic p-value', 'F-Statistic', 'F-Test p-value']

#print results of breuschpagan test
print(dict(zip(labels, breuschpagan_test)))

>>
{'Test Statistic': 178.0210735268372, 'Test Statistic p-value': 9.10528072676407e-24, 'F-Statistic': 9.060198266577352, 'F-Test p-value': 1.2161523288595614e-29}
```

Test 的結果為非常顯著，有 Heteroscedasticity。

### TS'5 Autocorrelation

Serial correlation，或叫做 autocorrelation，代表變數在資料集之間的相關程度，也就是兩次觀察之間的相似度對它們之間的時間差的函數。常被使用在 time series data 中 observations 有不同的時間點的情況下。

像是此報告中我們估計以太幣在一年之中每個月的幣價，如果較近的時間比較遠的時間更雷同，就稱作 correlated。

![](https://i.imgur.com/JBvOZeI.png)


### TS'5 Autocorrelation-Breusch-Godfrey Test

因為此資料為 weak exogenous，所以無法使用 Durbin-Watson Test。

```
import statsmodels.stats.diagnostic as dg

print(dg.acorr_breusch_godfrey(model, nlags=2))
>
(94.58755470641997, 2.264390338315469e-20, 35.68650816114991, 4.441467575871316e-21)
```

可以發現 Test statistic X2=94.58755470641997，P-value=2.264390338315469e-20，非常顯著！

---

## Model 2 - HAC vs. FGLS

### Newey-West standard errors for OLS - HAC

由上述 Test 我們知道可以使用：Newey-West standard errors for OLS，也就是 Heteroskedasticity and Autocorrelation in regression：

```
                      Newey-West OLS Regression Results                       
==============================================================================
Dep. Variable:               ETH/USDT   R-squared:                       0.992
Model:                            OLS   Adj. R-squared:                  0.992
Method:                 Least Squares   F-statistic:                     123.0
Date:                Fri, 27 May 2022   Prob (F-statistic):          1.53e-179
Time:                        04:51:29   Log-Likelihood:                -3509.7
No. Observations:                 579   AIC:                             7075.
Df Residuals:                     551   BIC:                             7198.
Df Model:                          27                                         
Covariance Type:                  HAC                                         
===============================================================================================
                                  coef    std err          z      P>|z|      [0.025      0.975]
-----------------------------------------------------------------------------------------------
const                        1.134e+04   4426.432      2.561      0.010    2661.666       2e+04
ETH_lag1                      759.4783     76.461      9.933      0.000     609.618     909.338
ETH_lag10                     -42.0006     49.869     -0.842      0.400    -139.742      55.741
ETH_lag20                      70.7987     43.025      1.646      0.100     -13.529     155.127
ETH_lag30                      23.3791     44.828      0.522      0.602     -64.482     111.240
EtherSupply                   -2.8e-05   2.43e-05     -1.150      0.250   -7.57e-05    1.97e-05
Daily Ether Burnt(ETH)         -0.0294      0.008     -3.521      0.000      -0.046      -0.013
Gas Limit                      -0.0003   6.99e-05     -3.833      0.000      -0.000      -0.000
Gas Price(Wei)              -2.833e-09   6.46e-10     -4.383      0.000    -4.1e-09   -1.57e-09
Gas Used Per Day             4.356e-08   1.12e-08      3.902      0.000    2.17e-08    6.54e-08
Average Txn Fee (USD)          17.6229      3.575      4.929      0.000      10.615      24.631
Blocks Per Day                 -0.4906      0.239     -2.057      0.040      -0.958      -0.023
Average Block Size(Bytes)      -0.0071      0.002     -3.895      0.000      -0.011      -0.004
BlockDifficulty                 0.4270      0.133      3.205      0.001       0.166       0.688
Hash Rate(GH/S)                -0.0046      0.002     -2.771      0.006      -0.008      -0.001
Cumulative Unique Addresses -1.387e-05    5.5e-06     -2.519      0.012   -2.47e-05   -3.08e-06
Network Utilization         -8963.8744   2121.127     -4.226      0.000   -1.31e+04   -4806.541
TransactionsCount Per Day       0.0008      0.000      4.663      0.000       0.000       0.001
DeFi_TVL                     1.032e-08   1.78e-09      5.806      0.000    6.84e-09    1.38e-08
Verified Contracts              0.0446      0.113      0.394      0.693      -0.177       0.266
StableCoins Value            -5.77e-09   3.12e-09     -1.848      0.065   -1.19e-08     3.5e-10
Sales NFT per Day            6.994e-09   2.16e-07      0.032      0.974   -4.16e-07     4.3e-07
Number of sales NFT per Day     0.0004      0.000      1.172      0.241      -0.000       0.001
NASDAQ                          0.0131      0.020      0.663      0.507      -0.026       0.052
BTC/USDT                        0.0024      0.003      0.940      0.347      -0.003       0.007
BNB/USDT                       -0.4237      0.232     -1.826      0.068      -0.879       0.031
SOL/USDT                       -0.0826      0.651     -0.127      0.899      -1.359       1.194
ADA/USDT                       -0.8266     50.656     -0.016      0.987    -100.110      98.457
ETC/USDT                        4.4632      1.192      3.744      0.000       2.126       6.800
==============================================================================
Omnibus:                       64.486   Durbin-Watson:                   1.403
Prob(Omnibus):                  0.000   Jarque-Bera (JB):              309.225
Skew:                           0.348   Prob(JB):                     7.12e-68
Kurtosis:                       6.512   Cond. No.                     2.44e+14
==============================================================================

```

### HAC 模型 Fitting 結果

![](https://i.imgur.com/yoJVwtl.png)

發現 Fitting 的結果表現差強人意。

### 各模型比較

* 如果我們沒有 SE，那 OLS 會比較好因為其為 consistent。
    * 如果解釋變數並非 strictly exogenous，那 FGLS 不僅會 inefficient，還會 inconsistent。
    * HAC 計算大樣本的 robust standard errors 會有不錯的成效
* Trade-off Consistency(Robust, NW Std.err OLS) <-> Efficiency(FGLS)


### Significant Features

使用 P-value 的排序來比較兩種演算法選出來的重要變數是否不同。

| HAC | p-value | FGLS | p-value |
| -------- | -------- | -------- | -------- |
|ETH_lag1|0.0|DeFi_TVL|0.0|
|DeFi_TVL|0.0|ETH_lag1|0.0|
|Average Txn Fee (USD)|0.0|Average Txn Fee (USD)|0.0|
|TransactionsCount Per Day|0.0|Gas Price(Wei)|0.0|
|Gas Price(Wei)|1e-05|Daily Ether Burnt(ETH)|0.0|
|Network Utilization|2e-05|TransactionsCount Per Day |0.0|
|Gas Used Per Day|0.0001| Average Block Size(Bytes)|0.0|
|Average Block Size(Bytes)|0.0001| ETC/USDT|0.0|
|Gas Limit|0.00013|Cumulative Unique Addresses |0.00013|
|ETC/USDT|0.00018| BlockDifficulty|0.00021|
|Daily Ether Burnt(ETH)|0.00043| Network Utilization|0.00038|
|BlockDifficulty|0.00135| BNB/USDT|0.00043|
|Hash Rate(GH/S)|0.0056|Gas Limit |0.00154|
|const|0.01043|Hash Rate(GH/S) |0.00196|
|Cumulative Unique Addresses|0.01176| Gas Used Per Day| 0.0021|

---

## Model 3 - Stationarity vs. Non-Stationarity

當一個資料集的統計數值，像是 mean, variance 和 autocorrelation 不會隨著時間而改變，就稱作 Stationarity。

大部分 time series datasets 都不是 stationary，因為都會有 non-stationary 的元素存在其中，像是 trends 或 economic cycles。但我們可以用 ‘stationarize’ 的方式來把原始資料轉為我們最後預測模型的素材。

最典型的兩種檢查 Stationarity 的方法就是 Visualization 和 Augmented Dickey-Fuller (ADF) Test.

### ADF Test.
![](https://i.imgur.com/3ycTnXq.png)
```
Test statistic = -2.004
P-value = 0.285
Critical values :
	1%: -3.441834071558759 - The data is not stationary with 99% confidence
	5%: -2.8666061267054626 - The data is not stationary with 95% confidence
	10%: -2.569468095872659 - The data is not stationary with 90% confidence
```

### Detrend

![](https://i.imgur.com/YulcbaU.png)

```
Test statistic = -7.482
P-value = 0.000
Critical values :
	1%: -3.441935806025943 - The data is  stationary with 99% confidence
	5%: -2.8666509204896093 - The data is  stationary with 95% confidence
	10%: -2.5694919649816947 - The data is  stationary with 90% confidence
```

![](https://i.imgur.com/nbFT5s5.png)

### Detrend-HAC

使用 Detrend 之後的資料做 HAC：
```
                      Newey-West OLS Regression Results                       
==============================================================================
Dep. Variable:               ETH/USDT   R-squared:                       0.561
Model:                            OLS   Adj. R-squared:                  0.540
Method:                 Least Squares   F-statistic:                     11.50
Date:                Fri, 27 May 2022   Prob (F-statistic):           2.22e-28
Time:                        04:51:51   Log-Likelihood:                -708.95
No. Observations:                 579   AIC:                             1474.
Df Residuals:                     551   BIC:                             1596.
Df Model:                          27                                         
Covariance Type:                  HAC                                         
===============================================================================================
                                  coef    std err          z      P>|z|      [0.025      0.975]
-----------------------------------------------------------------------------------------------
const                          60.3464     39.097      1.544      0.123     -16.282     136.975
ETH_lag1                        2.5626      0.935      2.740      0.006       0.729       4.396
ETH_lag10                      -4.7408      0.538     -8.818      0.000      -5.795      -3.687
ETH_lag20                       0.9534      0.499      1.911      0.056      -0.025       1.931
ETH_lag30                      -0.6520      0.504     -1.293      0.196      -1.641       0.337
EtherSupply                 -6.418e-09   1.93e-07     -0.033      0.973   -3.84e-07    3.71e-07
Daily Ether Burnt(ETH)      -8.086e-05   3.47e-05     -2.332      0.020      -0.000   -1.29e-05
Gas Limit                   -9.669e-07   7.91e-07     -1.223      0.221   -2.52e-06    5.83e-07
Gas Price(Wei)               -1.47e-11   3.31e-12     -4.436      0.000   -2.12e-11    -8.2e-12
Gas Used Per Day             1.403e-10    1.4e-10      0.999      0.318   -1.35e-10    4.16e-10
Average Txn Fee (USD)           0.0621      0.018      3.470      0.001       0.027       0.097
Blocks Per Day                 -0.0024      0.002     -1.134      0.257      -0.007       0.002
Average Block Size(Bytes)   -1.459e-05   1.29e-05     -1.132      0.258   -3.99e-05    1.07e-05
BlockDifficulty                 0.0004      0.001      0.421      0.674      -0.002       0.002
Hash Rate(GH/S)             -4.067e-06    1.2e-05     -0.338      0.735   -2.76e-05    1.95e-05
Cumulative Unique Addresses  -8.17e-08   3.62e-08     -2.260      0.024   -1.53e-07   -1.08e-08
Network Utilization           -30.2207     23.533     -1.284      0.199     -76.345      15.904
TransactionsCount Per Day     5.12e-06   7.81e-07      6.554      0.000    3.59e-06    6.65e-06
DeFi_TVL                     2.847e-11   9.16e-12      3.107      0.002    1.05e-11    4.64e-11
Verified Contracts             -0.0006      0.001     -0.825      0.409      -0.002       0.001
StableCoins Value            4.582e-11   2.45e-11      1.871      0.061   -2.17e-12    9.38e-11
Sales NFT per Day            9.157e-10   1.59e-09      0.576      0.564    -2.2e-09    4.03e-09
Number of sales NFT per Day  1.984e-07   2.13e-06      0.093      0.926   -3.97e-06    4.37e-06
NASDAQ                          0.0002      0.000      0.884      0.377      -0.000       0.001
BTC/USDT                     4.694e-05   2.06e-05      2.276      0.023    6.52e-06    8.73e-05
BNB/USDT                       -0.0074      0.002     -4.311      0.000      -0.011      -0.004
SOL/USDT                       -0.0167      0.004     -4.068      0.000      -0.025      -0.009
ADA/USDT                        0.8587      0.288      2.977      0.003       0.293       1.424
ETC/USDT                       -0.0183      0.007     -2.604      0.009      -0.032      -0.005
==============================================================================
Omnibus:                        3.410   Durbin-Watson:                   0.782
Prob(Omnibus):                  0.182   Jarque-Bera (JB):                3.455
Skew:                           0.186   Prob(JB):                        0.178
Kurtosis:                       2.929   Cond. No.                     2.44e+14
==============================================================================
```

### Significant Features

| HAC(Stationarity) | p-value | HAC(Non-Stationarity) | p-value |
| -------- | -------- | -------- | -------- |
|   ETH_lag10    |   0.0    |   ETH_lag1    |   0.0    |
|   Gas Price(Wei)    |   0.0    |   DeFi_TVL    |   0.0    |
|    TransactionsCount Per Day   |  0.0     |   Average Txn Fee (USD)    |  0.0     |
|    BNB/USDT   |   0.0    |    TransactionsCount Per Day   |    0.0   |
|   SOL/USDT    |  0.0     |  Gas Price(Wei)     |   1e-05    |
|   Average Txn Fee (USD)    |  0.001     |   Network Utilization    |   2e-05    |
|    DeFi_TVL   |   0.002    |   Gas Used Per Day    |     0.0001  |
|   ADA/USDT    |   0.003    |   Average Block Size(Bytes)    |    0.0001   |
|   ETH_lag1    |    0.006   |   Gas Limit    |   0.00013    |
|   ETC/USDT    |    0.009   |   ETC/USDT    |  0.00018     |
|   Daily Ether Burnt(ETH)    |  0.02     |  Daily Ether Burnt(ETH)     |    0.00043   |
|    BTC/USDT   |     0.023  |   BlockDifficulty    |    0.00135   |
|    Cumulative Unique Addresses   |   0.024    |  Hash Rate(GH/S)     |   0.0056    |
|   ETH_lag20    |    0.056   |       |       |
|    StableCoins Value   |   0.061    |       |       |


---

## Model 4 - Data Split

將資料切分成兩者：Before 10/9/2020 vs. After 10/9/2020

![](https://i.imgur.com/OOjx9qz.png)


### Data Before

![](https://i.imgur.com/kgTHIeL.png)
* 在切分日期以前還沒有某些變數的資料，例如燃燒機制、DeFi、NFT 等都還沒出現
* 和競爭幣的相關性降地很多
* 和擴充性服務（DeFi, NFT）的相關性降低，而和安全性（Hash Rate）的相關性提高


### Test TS'4 & TS'5

可以發現前 75% 的資料也有 Heteroskedasticity 和 Autocorrelation。

![](https://i.imgur.com/IIjgrgw.png)


![](https://i.imgur.com/QURBUak.png)

### The HAC of Data Before

```
Newey-West OLS Regression Results                       
==============================================================================
Dep. Variable:               ETH/USDT   R-squared:                       0.963
Model:                            OLS   Adj. R-squared:                  0.962
Method:                 Least Squares   F-statistic:                     84.36
Date:                Fri, 27 May 2022   Prob (F-statistic):          7.97e-235
Time:                        04:52:47   Log-Likelihood:                -9862.8
No. Observations:                1900   AIC:                         1.978e+04
Df Residuals:                    1872   BIC:                         1.994e+04
Df Model:                          27                                         
Covariance Type:                  HAC                                         
===============================================================================================
                                  coef    std err          z      P>|z|      [0.025      0.975]
-----------------------------------------------------------------------------------------------
const                         606.3436    132.589      4.573      0.000     346.475     866.212
ETH_lag1                       20.7280     10.584      1.958      0.050      -0.017      41.473
ETH_lag10                      17.2297     10.133      1.700      0.089      -2.631      37.091
ETH_lag20                       1.7771     14.353      0.124      0.901     -26.355      29.909
ETH_lag30                      -4.1038     12.472     -0.329      0.742     -28.549      20.341
EtherSupply                 -7.545e-06   1.76e-06     -4.289      0.000    -1.1e-05    -4.1e-06
Daily Ether Burnt(ETH)       5.178e-05   1.96e-05      2.639      0.008    1.33e-05    9.02e-05
Gas Limit                   -8.426e-06   4.81e-06     -1.752      0.080   -1.78e-05    9.98e-07
Gas Price(Wei)              -6.378e-11    3.8e-11     -1.678      0.093   -1.38e-10    1.07e-11
Gas Used Per Day             1.663e-09   1.53e-09      1.085      0.278   -1.34e-09    4.67e-09
Average Txn Fee (USD)          11.1453      5.695      1.957      0.050      -0.017      22.308
Blocks Per Day                 -0.0043      0.014     -0.306      0.759      -0.032       0.023
Average Block Size(Bytes)      -0.0022      0.002     -1.359      0.174      -0.005       0.001
BlockDifficulty                 0.0840      0.047      1.779      0.075      -0.009       0.176
Hash Rate(GH/S)                -0.0006      0.001     -1.102      0.270      -0.002       0.000
Cumulative Unique Addresses -1.369e-07   6.43e-07     -0.213      0.831    -1.4e-06    1.12e-06
Network Utilization          -136.9067     78.877     -1.736      0.083    -291.504      17.690
TransactionsCount Per Day       0.0002   5.79e-05      2.992      0.003    5.98e-05       0.000
DeFi_TVL                    -3.041e-09   5.32e-09     -0.572      0.567   -1.35e-08    7.38e-09
Verified Contracts              0.1927      0.096      2.011      0.044       0.005       0.380
StableCoins Value            7.348e-09   3.53e-09      2.084      0.037    4.38e-10    1.43e-08
Sales NFT per Day           -3.605e-05   1.77e-05     -2.038      0.042   -7.07e-05   -1.38e-06
Number of sales NFT per Day    -0.0008      0.001     -0.954      0.340      -0.003       0.001
NASDAQ                          0.0007      0.007      0.107      0.915      -0.012       0.014
BTC/USDT                        0.0012      0.003      0.376      0.707      -0.005       0.008
BNB/USDT                        2.2765      1.134      2.008      0.045       0.054       4.499
SOL/USDT                       -6.8378     14.210     -0.481      0.630     -34.688      21.013
ADA/USDT                      530.8346    140.969      3.766      0.000     254.541     807.128
ETC/USDT                       12.1794      1.813      6.720      0.000       8.627      15.732
==============================================================================

```

### Significant Features

| After 2021/1/1 | p-value | Before 2021/1/1 | p-value |
| -------- | -------- | -------- | -------- |
|ETH_lag1|0.0|ETC/USDT|0.0|
|DeFi_TVL|0.0|const|0.0|
|Average Txn Fee (USD)|0.0|EtherSupply|2e-05|
|TransactionsCount Per Day|0.0|ADA/USDT|0.00017|
|Gas Price(Wei)|1e-05|TransactionsCount Per Day|0.00278|
|Network Utilization|2e-05|Daily Ether Burnt(ETH)|0.00832|
|Gas Used Per Day|0.0001|StableCoins Value|0.03714|
|Average Block Size(Bytes)|0.0001|Sales NFT per Day|0.04152|
|Gas Limit|0.00013|Verified Contracts|0.0443|
|ETC/USDT|0.00018|BNB/USDT|0.04467|
|Daily Ether Burnt(ETH)|0.00043|ETH_lag1|0.05019|
|BlockDifficulty|0.00135|Average Txn Fee (USD)|0.05036|
|Hash Rate(GH/S)|0.0056|BlockDifficulty|0.07527|
|const|0.01043|Gas Limit|0.07972|
|Cumulative Unique Addresses|0.01176|Network Utilization|0.08262|

---


## 總結

### 結論

1. 與過去研究的比較
    * 過去研究發現比特幣和那斯達克指數與加密貨幣市場的相關性非常高，但現在有了去中心化金融服務之後，其他有相同服務的競爭鏈反而有更高的相關
1. 重要因子發現
    * 交易手續費、交易量、去中心化金融服務、競爭幣價格等，與以太幣價格的相關性無論在何種模型都非常顯著
1. 模型使用場景
    * 更了解 ETH/USDT 資料的趨勢和類型，包含 Stationarity 和 Hetero、Auto Corr 等資訊，藉此找到適合的分析模型


### 未來展望

1. Response Var.
    * 希望找到更多不同的辦法蒐集其他種類的的 y 資料，不一定是以 ETH/USDT 作為 Response Var.
1. Location Factor
    * 目前是以世界去區隔，未來朝以各國中心話交易所的風土民情方向處理模型資料，這樣可以找出交易所溢價的套利機會
1. Foundation Target & Regulations 
    * 實際上我們沒有辦法判斷 Ethereum Foundation 的策略和談話是利空還是利好，針對 EF 發表內容的性質分析對 Ether 的價值比較適合另外做一篇研究報告來討論。
    * 另外一個研究方向是關注 SEC 的談話，如果該月出現負面報導就訂為 1，出現正面報導、無相關報導，或者無法定義的報導都定義為 0。我相信這個相關性也很高只是資料蒐集的負擔比較大，可能需要花一點時間做文字探勘所以也省略。

---

## Reference

* [BLOCK EXPLORERS](https://ethereum.org/en/developers/docs/data-and-analytics/block-explorers/)
* [DATA AND ANALYTICS](https://ethereum.org/en/developers/docs/data-and-analytics/)
* [On Inflation, Transaction Fees and Cryptocurrency Monetary Policy](https://blog.ethereum.org/2016/07/27/inflation-transaction-fees-cryptocurrency-monetary-policy/?fbclid=IwAR3B_0O7r9rZ5EQYmPbygaAzzOl2frlasrZEGf1EyCiK17PitaFNjwuVWHo)

### Data Collected From

* [Etherscan](https://etherscan.io/)
* [Defi Llama](https://defillama.com/)
* [Nonfungible](http://nonfungible.com/market-tracker)
* [Coingecko](https://www.coingecko.com/)
* [Coinresources](https://www.coinresources.io/)
* [Tokenterminal](https://tokenterminal.com/terminal/projects/ethereum?gclid=Cj0KCQjwg_iTBhDrARIsAD3Ib5i1QEtc99avjBe6yiFA34VNyupvKTelw-G6gQBCI4P1PPiH8Ts_hSMaAqj-EALw_wcB)
* [blockchain.com](https://www.blockchain.com/charts)

### Time Series in Python
* [Time Series Analysis in Python – A Comprehensive Guide with Examples](https://www.machinelearningplus.com/time-series/time-series-analysis-python/)
* [Topic 9. Part 1. Time series analysis in Python](https://www.kaggle.com/code/kashnitsky/topic-9-part-1-time-series-analysis-in-python/notebook#Econometric-approach)
* [A Guide to Time Series Forecasting in Python](https://builtin.com/data-science/time-series-forecasting-python)
* [Forecasting with a Time Series Model using Python: Part One](https://www.bounteous.com/insights/2020/09/15/forecasting-time-series-model-using-python-part-one/)
* [Multiple Linear Regression](https://www.codecademy.com/learn/linear-regression-mssp/modules/multiple-linear-regression-mssp/cheatsheet)
* [4.8 Autocorrelated (Serially Correlated) Errors](http://web.vu.lt/mif/a.buteikis/wp-content/uploads/PE_Book/4-8-Multiple-autocorrelation.html)

### Time Series in R
* [A Guide to Time Series Forecasting in R You Should Know](https://www.simplilearn.com/tutorials/data-science-tutorial/time-series-forecasting-in-r)
* [Exploring Four Simple Time Series Forecasting Methods with R Examples](https://www.mssqltips.com/sqlservertip/6778/time-series-forecasting-methods-with-r-examples/)
* [R Statistics - Time Series Forecasting](http://r-statistics.co/Time-Series-Forecasting-With-R.html)
* [Quick-R - Time Series and Forecasting](https://www.statmethods.net/advstats/timeseries.html)
* [Forcasting-A-Time-Series-Stock-Market-Data](https://stat-wizards.github.io/Forcasting-A-Time-Series-Stock-Market-Data/)
* [A Complete Tutorial on Time Series Modeling in R](https://www.analyticsvidhya.com/blog/2015/12/complete-tutorial-time-series-modeling/)
* [Understanding Time Series with R](https://www.kdnuggets.com/2020/07/understanding-time-series-r.html)

### Evaluating ETH Price
* [The fair value of a token: How do markets price cryptocurrencies?](https://www.sciencedirect.com/science/article/pii/S0275531919300601)
* [Ethereum Valuation](https://www.macroaxis.com/valuation/ETH.CC/Ethereum)
* [Quantifying Blockchain Extractable Value:How dark is the forest?](https://arxiv.org/pdf/2101.05511.pdf)
