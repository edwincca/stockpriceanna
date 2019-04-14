# stockpriceanna
a collection of tools for analyzing stock prices

current version: 1.0.1

installation:
pip install stockpriceanna (or pip install stockpriceanna --upgrade)
required package: pandas, numpy


Completed projects
======================================================================================================================================
VR_test:
----------------------------
    This method take a array-like data as input and perform variance ratio test according to Lo and MacKinlay's algorithem for testing the hypothsis that price follows gemetric random walk
    The test detects deviation from log-normal distribution in asset returns by comparing estimated variances from different holding periods
    This method allows array-like inputs for the choise of long-period variance and it returns the Z statistics or the p-values corresponding to each input

example:
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



