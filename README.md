# stockpriceanna
a collection of tools for analyzing stock prices

current version: 1.0.7

installation:
```
pip install stockpriceanna 
```
or
```
pip install stockpriceanna --upgrade
```
required package: pandas, numpy

Completed projects
===========================================================================================================================
VR_test:
----------------------------

This method takes a array-like data as input and perform variance ratio test according to Lo and MacKinlay's algorithem for testing the hypothsis that price follows gemetric random walk

The test detects deviation from log-normal distribution in asset returns by comparing estimated variances from different holding periods

This method allows array-like inputs for the choise of long-period variance and it returns the Z statistics or the p-values corresponding to each input

example:
```
from stockpriceanna import VR_test
from pandas_datareader import data

p1= data.DataReader('AAPL', 'yahoo', "2013-01-01", "2019-01-01")  #a super large cap stock
VR_test(data=p1["Adj Close"], sub_size=range(2,10))

output:  (do not reject the null)
lvar_size
2    0.3264
3   -0.0111
4   -0.6196
5    0.4911
6   -0.6539
7   -0.3045
8   -0.9522
9    0.2282
Name: Z-stat, dtype: float64

p2= data.DataReader('ACLS', 'yahoo', "2013-01-01", "2019-01-01") #a small cap stock
VR_test(data=p1["Adj Close"], sub_size=range(2,10))
output:  (some evidences for rejecting the null)
lvar_size
2   -1.4314
3   -1.4439
4   -2.3415
5   -2.0108
6   -2.9587
7   -2.6798
8   -1.9327
9   -2.7329
Name: Z-stat, dtype: float64
```

pricegen
--------------------------------
The pricegen class generates a simulated price series made of two components - the real and the noise. The noise component has only an one-period effect on price while the real component has a permanent effect on price, which leads to a random walk. User can further specify a determinsitic trend factor and a decaying effect of random shock on stock prices, which creates artificial autocorrelation in returns. The generated factor and noise components are stored in the self.data dataframe for analysis from an insider's view. 

example:
```
from stockpriceanna.pricegen import spgen

d1 = spgen(size=1000)  #specify the size of the simulation
d1.gen_ln(var=0.01,seed=1) #create a log-normal component as a factor
d1.gen_ln(mean=0,var=0.005,add_type="noise",seed=2) #create a log-normal component as a noise
d1.gen_ac(base="ln_u=0_v=0.01",rate=0.2,lag=3) #specify how previous shock to stock price decay overtime
d1.gen_trend(rate=0.0005) #set a trend 
d1.gen_custom(func =lambda:(1+ 0.02*(np.random.rand()-0.5)),name="unit_noise",add_type="noise") #create another noise variable that follows unitform distribution
d1.gen_custom(func = lambda l1,l2: (l1+l2)/2 if (l1-1)*(l2-1)>0 else 1,var="unit_noise",name="mom_factor",add_type="factor") #create a momentum factor based on the lag values of the uniform noise
d1.gen_price(method="m") #generate the stock prices using multiplicative model
d1.show_price(show_rprice=True).tail(10)

output:
                price     price_r
2016-07-20  99.791838  100.000000
2016-07-21  99.737430   99.765493
2016-07-22  98.179226   99.233497
2016-07-23  98.916888   98.108953
2016-07-24  97.892366   98.774131

d1.show_price(show_rprice=True).tail(5)
output:
                 price     price_r
2019-04-11  272.547973  272.436344
2019-04-12  267.286253  266.527270
2019-04-13  264.979419  265.290266
2019-04-14  266.131092  266.084376
2019-04-15  266.115019  265.853310
```


