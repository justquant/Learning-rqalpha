Talib是一个广泛使用的技术分析指标库。提供了多种语言的接口。但是据说它的python API封装是最友好的。
本文的介绍基于Python。

## Talib的功能/指标分类

* Overlap Studies 重叠研究
* Momentum Indicators 动量指标
* Volume Indicators 成交量指标
* Volatility Indicators 波动率指标
* Price Transform 价格转换
* Cycle Indicators  周期指标
* Pattern Recognition 模式识别
* Statistic Function  统计函数
* Math Transform  数学转换
* Math Operators  数学操作符

## 重叠研究

这一个分类下的指标有：

* BBANDS - Bollinger Bands 布林带
* DEMA - Double Exponential Moving Average 双指数移动平均线
* EMA - Exponential Moving Average 指数移动平均线
* HT_TRENDLINE - Hilbert Transform - Instantaneous Trendline 希尔伯特变换 - 瞬时趋势线
* KAMA - Kaufman Adaptive Moving Average 考夫曼自适应移动平均线
* MAMA - MESA Adaptive Moving Average  MESA自适应移动平均线
* MA - Moving Average 移动平均线。
* MAVP - Moving average with variable period 可变区间的移动平均线
* MIDPOINT - MidPoint over period  区间中位数
* MIDPRICE - Midpoint Price over period 区间中位价
* SAR - Parabolic SAR 抛物线SAR
* SAREXT - Parabolic SAR Extended SAR的扩展指标
* SMA - Simple Moving Average 简单移动平均线
* T3 - Triple Exponential Moving Average 三重指数移动平均
* TEMA - Triple Exponential Moving Average 三重指数移动平均
* TRIMA - Triangular Moving Average 三角加权移动平均线
* WMA - Weighted Moving Average 加权移动平均线

### 布林带 （BBANDS）

关于布林带的介绍请参考这篇[文章](https://baike.baidu.com/item/%E4%BF%9D%E5%88%A9%E5%8A%A0%E9%80%9A%E9%81%93/7355829?fr=aladdin&fromid=8253987&fromtitle=%E5%B8%83%E6%9E%97%E5%B8%A6)

布林线指标的实现原理是标准差。从统计学的角度看，股价总是围绕一个价值中枢（均线）上下波动。基于这一理念，布林线引入了股价通道的概念，认为股价通道的宽窄随着股价波动幅度的变化而变化。其有三个要素：

中间线: 一般为20日均线
上轨线(Up)： 中间线加上它的2倍标准差
下轨先(Down): 中间线减去它的2倍标准差

注意talib的BBANDS函数的timeperiod默认是5，为了获得和腾讯自选股软件一样的结果，需要将timeperiod指定为20，mattype位简单移动平均线。
```python
upper, middle, lower = talib.BBANDS(close,timeperiod=20, nbdevup=2, nbdevdn=2, matype=MA_Type.SMA)
```

### EMA 指数移动平均线

百度百科参考资料: https://baike.baidu.com/item/EMA/12646151?fr=aladdin

相对于简单平均SMA，EMA对当前的价格加大了比重。当要比较数值与均价的关系时，用MA就可以了，而要比较均价的趋势快慢时，用EMA更稳定；这就是为什么MACD指标要采用EMA作为基础的原因。

Talib函数：

    real = EMA(close, timeperiod=30)

### DEMA 双重指数移动平均线

参考资料：http://blog.sina.com.cn/s/blog_7542a31c0101auvj.html

计算公式：

    DEMA = 2 * EMA(input) - EMA(EMA(input))

Talib函数：

    real = DEMA(close, timeperiod=30)

有研究认为，DEMA比MACD更加有效。

### 希尔伯特变换 - 瞬时趋势线

通过对价格进行希尔伯特变换，得到一个趋势线。使用方法：

    real = HT_TRENDLINE(close)

API说有一个unstable period, 这个不太了解

### TEMA 三重指数移动平均线

TEMA是三重指数平均线的一种形式。

计算公式：

    TEMA = 3*EMA(input) - 3 * EMA(EMA(input)) + EMA(EMA(EMA(input)))

Talib函数：

    real = TEMA(close, timeperiod=30)

TEAM参考资料：http://www.fmlabs.com/reference/default.htm?url=TEMA.htm


### T3 三重指数平滑平均线

T3是一种更加平滑和敏感的移动平均线。 

计算公式:

    GD(series) = EMA(series) * (1+vfactor) - EMA(EMA(series)) * vfactor
    T3 = GD(GD(GD(series)))

Talib 函数：

    real = T3(close, timeperiod=5, vfactor=0)

vfactor对于均线的效果有很大影响。一般建议vfactor=0.7 (有考虑过0.618？)

T3 参考资料：http://www.fmlabs.com/reference/default.htm?url=T3.htm

### KAMA 考夫曼自适应移动平均线

短期均线太敏感，长期均线太滞后，于是有了考夫曼自适应移动平均线。KAMA的优点是具有自适应性，当趋势明显时，KAMA紧随价格而变动，类似于短周期均线；当价格横向摆动时，KAMA走平，类似于长周期均线。

Talib函数：

    real = KAMA(close, timeperiod=30)

### MAMA 

参考资料：http://www.binarytribune.com/forex-trading-indicators/ehlers-mesa-adaptive-moving-average

MAMA是一个准确率很好的指标，它产生两条线：快线MAMA和慢线FAMA。可以使用类似于MACD的指标来使用MAMA。

### MIDPOINT 平均价位

（最高价+最低价）/ 2

Talib函数：

    real = MIDPOINT(close, timeperiod=14)

### MIDPRICE

计算公式：
    
    (highest high + lowest low)/2

Talib函数：

    real = MIDPRICE(high, low, timeperiod=14)

### Parabolic SAR 抛物线SAR

参考资料：http://www.investopedia.com/articles/technical/02/042202.asp

根据上面的文章，这个指标在趋势明显时非常有效，在震荡市时经常发出假信号。但是这个指标是一个非常好的趋势跟踪用于止损的指标。

Talib函数：
    * real = SAR(high, low, acceleration=0, maximum=0)
    * real = SAREXT(high, low, startvalue=0, offsetonreverse=0, accelerationinitlong=0, accelerationlong=0, accelerationmaxlong=0, accelerationinitshort=0, accelerationshort=0, accelerationmaxshort=0)

### TRIMA 三角加权移动平均线

参考文章：[来自thebalance](https://www.thebalance.com/triangular-moving-average-tma-description-and-uses-1031203)

一种计算均线时，对中间的价格赋予更大比重的计算方式。它严重滞后于价格，是否可以用来止损？

目前还没有找到应用的场景。

### WMA 加权移动平均线
作用和EMA非常类似。计算方式参考：http://www.fmlabs.com/reference/default.htm?url=WeightedMA.htm

## 动量指标函数

### ADX - Average Directional Movement Index 

ADX本身不表示趋势，而是表示趋势的强度。它的值介于 0 - 100 之间。

ADX值|趋势强度
-------|---------|
0 - 25 |没有趋势或者很弱的趋势
25 - 50 | 较强的趋势
50 - 75 | 很强的趋势
75 - 100 | 极端强的趋势

参考文章：

* [投资百科的ADX介绍,非常全面](http://www.investopedia.com/articles/trading/07/adx-trend-indicator.asp?ad=dirN&qo=investopediaSiteSearch&qsrc=0&o=40186)

### ADXR - Average Directional Movement Index Rating



## 资料引用

* [Talib的Python封装](http://mrjbq7.github.io/ta-lib/)
* [投资类的维基百科](http://www.investopedia.com)