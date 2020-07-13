# stock price analytical tools
## current version: 1.2.2

## installation
> pip install stockpriceanna (or pip install stockpriceanna --upgrade)

## required package
> pandas, numpy


## Completed projects

## VR_test:
>   This method takes a array-like data as input and perform variance ratio test according to Lo and MacKinlay's algorithem for testing the hypothsis that price follows gemetric random walk
>   The test detects deviation from log-normal distribution in asset returns by comparing estimated variances from different holding periods
>   This method allows array-like inputs for the choise of long-period variance and it returns the Z statistics or the p-values corresponding to each input

## example:
```sh
from stockpriceanna import VR_test

sp500 = pd.read_csv(
"https://query1.finance.yahoo.com/v7/finance/download/%5EGSPC?period1=946684800&period2=1577836800&interval=1d&events=history") 
#load SP500 index daily data betwen 2000-2020 from Yahoo 
sp500 =sp500.set_index("Date",drop=True)
p1 = sp500.loc[:"2010"]["Close"] 
p2 = sp500.loc["2010":]["Close"]
VR_test(data=p1, periods=range(2,10))
output:  
periods
2   -2.599856
3   -3.200185
4   -2.743059
5   -2.502471
6   -2.406569
7   -2.287762
8   -2.262762
9   -2.167513
Name: Z-stat, dtype: float64
#comment: strong evidence of mean reversion

VR_test(data=p2, periods=range(2,10))
output: 
periods
2   -1.413111
3   -1.097611
4   -1.224649
5   -1.272945
6   -1.582837
7   -1.727371
8   -1.756683
9   -1.817293
Name: Z-stat, dtype: float64
#comment: minor but not enough evidence of mean reversion. the U.S market appears to become more efficient
'''

## pricegen
The pricegen class generates a simulated price series made of two components - the real and the noise. The noise component has only an one-period effect on price while the real component has a permanent effect on price, which leads to a random walk. User can further specify a determinsitic trend factor and a decaying effect of random shock on stock prices, which creates artificial autocorrelation in returns. The generated factor and noise components are stored in the self.data dataframe for analysis from an insider's view.

## example
'''sh
from stockpriceanna.pricegen import spgen

d1 = spgen(size=1000)  #specify the size of the simulation
d1.gen_ln(std=0.01,seed=1) #create a log-normal component as a factor
d1.gen_ln(mean=0,std=0.005,add_type="noise",seed=2) #create a log-normal component as a noise
d1.gen_ac(base="ln_u=0_v=0.01",rate=0.2,lag=3) #specify how previous shock to stock price decay overtime
d1.gen_trend(rate=0.0005) #set a trend 
d1.gen_price() #generate the stock price
d1.show_price(show_rprice=True).head(5)
output:
                 price     price_r
2017-10-17  100.000000  100.000000
2017-10-18   99.737430   99.765493
2017-10-19   98.179226   99.233497
2017-10-20   98.916888   98.108953
2017-10-21   97.892366   98.774131

d1.show_price(show_rprice=True).tail(5)
output:
                 price     price_r
2020-07-08  272.547973  272.436344
2020-07-09  267.286253  266.527270
2020-07-10  264.979419  265.290266
2020-07-11  266.131092  266.084376
2020-07-12  266.115019  265.853310
'''
