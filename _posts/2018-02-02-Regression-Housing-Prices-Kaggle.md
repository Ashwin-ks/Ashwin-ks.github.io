---
layout: post
title: Regression-Advanced Housing Prices Kaggle
subtitle: Python implementation of Housing Prices prediction competition from Kaggle
bigimg: /img/housebanner.png
---

## Housing Prices - Regression - Kaggle
Predict sales prices and practice feature engineering, RFs, and gradient boosting

Ask a home buyer to describe their dream house, and they probably won't begin with the height of the basement ceiling or the proximity to an east-west railroad. But this playground competition's dataset proves that much more influences price negotiations than the number of bedrooms or a white-picket fence.

With 79 explanatory variables describing (almost) every aspect of residential homes in Ames, Iowa, this competition challenges you to predict the final price of each home.

```python
#main modules
import numpy as np 
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.rcParams['figure.figsize'] = (10.0, 8.0)
import seaborn as sns
from scipy import stats
from scipy.stats import norm
```

### Load the csv data into dataframes


```python
#read the data from csv files into dataframes
train_df = pd.read_csv('train.csv')
test_df = pd.read_csv('test.csv')
```


```python
#print out no of rows and columns in Train and test data
print('Train data:-\n Columns: {}  Rows: {}'.format(train_df.shape[1],train_df.shape[0]))
print('Test data:-\n Columns: {}  Rows: {}'.format(test_df.shape[1],test_df.shape[0]))
```

    Train data:-
     Columns: 81  Rows: 1460
    Test data:-
     Columns: 80  Rows: 1459
    


```python
train_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>5</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>60</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>12</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>250000</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 81 columns</p>
</div>




```python
test_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1461</td>
      <td>20</td>
      <td>RH</td>
      <td>80.0</td>
      <td>11622</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>120</td>
      <td>0</td>
      <td>NaN</td>
      <td>MnPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>6</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1462</td>
      <td>20</td>
      <td>RL</td>
      <td>81.0</td>
      <td>14267</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Gar2</td>
      <td>12500</td>
      <td>6</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1463</td>
      <td>60</td>
      <td>RL</td>
      <td>74.0</td>
      <td>13830</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>MnPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>3</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1464</td>
      <td>60</td>
      <td>RL</td>
      <td>78.0</td>
      <td>9978</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>6</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1465</td>
      <td>120</td>
      <td>RL</td>
      <td>43.0</td>
      <td>5005</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>HLS</td>
      <td>AllPub</td>
      <td>...</td>
      <td>144</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 80 columns</p>
</div>




```python
#combine both train and test data
#df=train_df.append(test_df,ignore_index=True)
df=pd.concat([train_df,test_df])
```


```python
df.shape
```




    (2919, 81)



### Get feel of data


```python
##only numeric columns
df.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1stFlrSF</th>
      <th>2ndFlrSF</th>
      <th>3SsnPorch</th>
      <th>BedroomAbvGr</th>
      <th>BsmtFinSF1</th>
      <th>BsmtFinSF2</th>
      <th>BsmtFullBath</th>
      <th>BsmtHalfBath</th>
      <th>BsmtUnfSF</th>
      <th>EnclosedPorch</th>
      <th>...</th>
      <th>OverallQual</th>
      <th>PoolArea</th>
      <th>SalePrice</th>
      <th>ScreenPorch</th>
      <th>TotRmsAbvGrd</th>
      <th>TotalBsmtSF</th>
      <th>WoodDeckSF</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>YrSold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>2918.000000</td>
      <td>2918.000000</td>
      <td>2917.000000</td>
      <td>2917.000000</td>
      <td>2918.000000</td>
      <td>2919.000000</td>
      <td>...</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>1460.000000</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>2918.000000</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
      <td>2919.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1159.581706</td>
      <td>336.483727</td>
      <td>2.602261</td>
      <td>2.860226</td>
      <td>441.423235</td>
      <td>49.582248</td>
      <td>0.429894</td>
      <td>0.061364</td>
      <td>560.772104</td>
      <td>23.098321</td>
      <td>...</td>
      <td>6.089072</td>
      <td>2.251799</td>
      <td>180921.195890</td>
      <td>16.062350</td>
      <td>6.451524</td>
      <td>1051.777587</td>
      <td>93.709832</td>
      <td>1971.312778</td>
      <td>1984.264474</td>
      <td>2007.792737</td>
    </tr>
    <tr>
      <th>std</th>
      <td>392.362079</td>
      <td>428.701456</td>
      <td>25.188169</td>
      <td>0.822693</td>
      <td>455.610826</td>
      <td>169.205611</td>
      <td>0.524736</td>
      <td>0.245687</td>
      <td>439.543659</td>
      <td>64.244246</td>
      <td>...</td>
      <td>1.409947</td>
      <td>35.663946</td>
      <td>79442.502883</td>
      <td>56.184365</td>
      <td>1.569379</td>
      <td>440.766258</td>
      <td>126.526589</td>
      <td>30.291442</td>
      <td>20.894344</td>
      <td>1.314964</td>
    </tr>
    <tr>
      <th>min</th>
      <td>334.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>34900.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1872.000000</td>
      <td>1950.000000</td>
      <td>2006.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>876.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>220.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>5.000000</td>
      <td>0.000000</td>
      <td>129975.000000</td>
      <td>0.000000</td>
      <td>5.000000</td>
      <td>793.000000</td>
      <td>0.000000</td>
      <td>1953.500000</td>
      <td>1965.000000</td>
      <td>2007.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1082.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>368.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>467.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>6.000000</td>
      <td>0.000000</td>
      <td>163000.000000</td>
      <td>0.000000</td>
      <td>6.000000</td>
      <td>989.500000</td>
      <td>0.000000</td>
      <td>1973.000000</td>
      <td>1993.000000</td>
      <td>2008.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1387.500000</td>
      <td>704.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>733.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>805.500000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>7.000000</td>
      <td>0.000000</td>
      <td>214000.000000</td>
      <td>0.000000</td>
      <td>7.000000</td>
      <td>1302.000000</td>
      <td>168.000000</td>
      <td>2001.000000</td>
      <td>2004.000000</td>
      <td>2009.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5095.000000</td>
      <td>2065.000000</td>
      <td>508.000000</td>
      <td>8.000000</td>
      <td>5644.000000</td>
      <td>1526.000000</td>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>2336.000000</td>
      <td>1012.000000</td>
      <td>...</td>
      <td>10.000000</td>
      <td>800.000000</td>
      <td>755000.000000</td>
      <td>576.000000</td>
      <td>15.000000</td>
      <td>6110.000000</td>
      <td>1424.000000</td>
      <td>2010.000000</td>
      <td>2010.000000</td>
      <td>2010.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 38 columns</p>
</div>




```python
#train_df.info()
def variable_dtype_plot(df):
    '''bar plot indicating the count of data types
        present in the dataframe'''
    df_dtype = pd.DataFrame(df.dtypes.value_counts()).reset_index().rename(columns={"index":"datatype",0:"count"})
    fig,ax = plt.subplots()
    fig.set_size_inches(20,5)
    sns.barplot(data=df_dtype,x="datatype",y="count",ax=ax,color="#34495e")
    ax.set(xlabel='Variable Type', ylabel='Count',title="Variables Count Across Datatype")
    plt.show()
```


```python
variable_dtype_plot(df)
```


![png](output_12_0.png)



```python
#check no of each column types
df.dtypes.value_counts()
```




    object     43
    int64      26
    float64    12
    dtype: int64




```python
#get numerical and categorical columns of dataframe into lists.
categorical = list(df.columns[df.dtypes=="object"])
numerical = list(df.columns[df.dtypes!="object"])
```


```python
#use describe method on the categorical columns
df[categorical].describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Alley</th>
      <th>BldgType</th>
      <th>BsmtCond</th>
      <th>BsmtExposure</th>
      <th>BsmtFinType1</th>
      <th>BsmtFinType2</th>
      <th>BsmtQual</th>
      <th>CentralAir</th>
      <th>Condition1</th>
      <th>Condition2</th>
      <th>...</th>
      <th>MiscFeature</th>
      <th>Neighborhood</th>
      <th>PavedDrive</th>
      <th>PoolQC</th>
      <th>RoofMatl</th>
      <th>RoofStyle</th>
      <th>SaleCondition</th>
      <th>SaleType</th>
      <th>Street</th>
      <th>Utilities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>198</td>
      <td>2919</td>
      <td>2837</td>
      <td>2837</td>
      <td>2840</td>
      <td>2839</td>
      <td>2838</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>...</td>
      <td>105</td>
      <td>2919</td>
      <td>2919</td>
      <td>10</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2918</td>
      <td>2919</td>
      <td>2917</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>6</td>
      <td>4</td>
      <td>2</td>
      <td>9</td>
      <td>8</td>
      <td>...</td>
      <td>4</td>
      <td>25</td>
      <td>3</td>
      <td>3</td>
      <td>8</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Grvl</td>
      <td>1Fam</td>
      <td>TA</td>
      <td>No</td>
      <td>Unf</td>
      <td>Unf</td>
      <td>TA</td>
      <td>Y</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>...</td>
      <td>Shed</td>
      <td>NAmes</td>
      <td>Y</td>
      <td>Gd</td>
      <td>CompShg</td>
      <td>Gable</td>
      <td>Normal</td>
      <td>WD</td>
      <td>Pave</td>
      <td>AllPub</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>120</td>
      <td>2425</td>
      <td>2606</td>
      <td>1904</td>
      <td>851</td>
      <td>2493</td>
      <td>1283</td>
      <td>2723</td>
      <td>2511</td>
      <td>2889</td>
      <td>...</td>
      <td>95</td>
      <td>443</td>
      <td>2641</td>
      <td>4</td>
      <td>2876</td>
      <td>2310</td>
      <td>2402</td>
      <td>2525</td>
      <td>2907</td>
      <td>2916</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 43 columns</p>
</div>



## Exploratory Data Analysis

analytically exploring data in order to provide some insights for subsequent processing and modeling.

Usually we would load the data using Pandas and make some visualizations to understand the data.

Inspect the distribution of target variable. Depending on what scoring metric is used, an imbalanced distribution of target variable might harm the model’s performance.  
For numerical variables, use box plot and scatter plot to inspect their distributions and check for outliers.  
For classification tasks, plot the data with points colored according to their labels. This can help with feature engineering.  
Make pairwise distribution plots and examine their correlations.  

https://www.kaggle.com/benhamner/d/uciml/iris/python-data-visualizations



```python
#univariate analysis
##plotting histograms to find distribution of continuous variables in dataset
#num = [f for f in df.columns if df.dtypes[f] != 'object']
numdf=pd.melt(train_df,value_vars=numerical)
numgrid=sns.FacetGrid(numdf,col='variable',col_wrap=5,sharex=False,sharey=False)
numgrid=numgrid.map(sns.distplot,'value')
numgrid
```




    <seaborn.axisgrid.FacetGrid at 0x210818eb080>




![png](output_17_1.png)



```python
##count plot of categorical attributes
#col = [f for f in train_df.columns if train_df.dtypes[f] == 'object']
coldf=pd.melt(train_df,value_vars=categorical)
colgrid=sns.FacetGrid(coldf,col='variable',col_wrap=4,size=6,aspect=.6,sharex=False,sharey=False)
plt.xticks(rotation=90) 
colgrid = colgrid.map(sns.countplot,'value')
plt.tight_layout()
plt.subplots_adjust(hspace=0.5, wspace=0.4)


###rotating x axis labels to prevent overlapping
for ax in colgrid.axes:
    for label in ax.get_xticklabels():
        label.set_rotation(90)
colgrid
```




    <seaborn.axisgrid.FacetGrid at 0x210859a5320>




![png](/regression_housing_prices_files/output_18_1.png)



```python
def boxplot(x,y,**kwargs):
            sns.boxplot(x=x,y=y)
            x = plt.xticks(rotation=90)

#cat = [f for f in train_df.columns if train_df.dtypes[f] == 'object']

p = pd.melt(train_df, id_vars='SalePrice', value_vars=categorical)
g = sns.FacetGrid (p, col='variable', col_wrap=2, sharex=False, sharey=False, size=5)
g = g.map(boxplot, 'value','SalePrice')
g
```




    <seaborn.axisgrid.FacetGrid at 0x21083ae2898>




![png](/regression_housing_prices_files/output_19_1.png)


### Data Preprocessing

Note that we combined the data from our training and test sets into a single dataframe here (called df), dropping our target variable (SalePrice). These numbers thus represent the total number of missing values across the full dataset. Since the total number of entries in our train and test sets is 2919, we can see that for some features nearly all entries are missing, while for others it is just one or two. How we proceed to treat these missing values depends very much on the reasons the data is missing, the problem and the type of model we want to use.

#### Checking for null values ,missing data


```python
#sns.pairplot(train_df[['LotFrontage','LotArea']],diag_kind="hist")
#check the columns which are having any null values
#train_df.columns[train_df.isnull().any()]
print('    >>>>>Numerical variables having nulls and its counts<<<<<<<<')
num_nulls = df[numerical].isnull().sum()[df[numerical].isnull().sum()>0].sort_values(ascending=False)
num_nulls
```

        >>>>>Numerical variables having nulls and its counts<<<<<<<<
    




    SalePrice       1459
    LotFrontage      486
    GarageYrBlt      159
    MasVnrArea        23
    BsmtHalfBath       2
    BsmtFullBath       2
    TotalBsmtSF        1
    GarageCars         1
    GarageArea         1
    BsmtUnfSF          1
    BsmtFinSF2         1
    BsmtFinSF1         1
    dtype: int64




```python
print('      >>>>>Categorical variables having nulls and its counts<<<<<<<<')
col_nulls = df[categorical].isnull().sum()[df[categorical].isnull().sum()>0].sort_values(ascending=False)
col_nulls
```

          >>>>>Categorical variables having nulls and its counts<<<<<<<<
    




    PoolQC          2909
    MiscFeature     2814
    Alley           2721
    Fence           2348
    FireplaceQu     1420
    GarageQual       159
    GarageFinish     159
    GarageCond       159
    GarageType       157
    BsmtCond          82
    BsmtExposure      82
    BsmtQual          81
    BsmtFinType2      80
    BsmtFinType1      79
    MasVnrType        24
    MSZoning           4
    Utilities          2
    Functional         2
    Electrical         1
    Exterior1st        1
    Exterior2nd        1
    SaleType           1
    KitchenQual        1
    dtype: int64



##### Treating Null values

We could either delete,impute or leave null values based on algorithms used.

On checking the distribution of 'LotFrontage' variable in below univariate analysis hist plot,we can see that data is skewed left,therefore we use the median value to impute missing value here


```python
df['LotFrontage'].fillna(df['LotFrontage'].median(),inplace=True)
```

We will fill 'GarageYrBlt' variable null values with zero for now as it indicates no garage built.  

If we look at the data description,missing values indicates meaningful data and we need to fill with 0 instead.

This might be used to create new feature in feature engineering.


```python
df['GarageYrBlt'].fillna(0, inplace=True)
```


```python
df.MasVnrArea.fillna(0, inplace=True)    
df.BsmtHalfBath.fillna(0, inplace=True)
df.BsmtFullBath.fillna(0, inplace=True)
df.GarageArea.fillna(0, inplace=True)
df.GarageCars.fillna(0, inplace=True)    
df.TotalBsmtSF.fillna(0, inplace=True)   
df.BsmtUnfSF.fillna(0, inplace=True)     
df.BsmtFinSF2.fillna(0, inplace=True)    
df.BsmtFinSF1.fillna(0, inplace=True)   
```

Now we have imputed missing values in numerical variables.Lets check the categorical variables null values.In case of PoolQC we can drop this whole column due to large null values and infering less dependancy with target variable.For now we will impute 'NA' as pool area for these is also 0.


```python
df.loc[df['PoolQC'].isnull()==True,['PoolArea','PoolQC']].describe()
df.PoolQC.fillna('NA', inplace=True)
```


```python
df.MiscFeature.fillna('NA', inplace=True)    
df.Alley.fillna('NA', inplace=True)          
df.Fence.fillna('NA', inplace=True)         
df.FireplaceQu.fillna('NA', inplace=True)    
df.GarageCond.fillna('NA', inplace=True)    
df.GarageQual.fillna('NA', inplace=True)     
df.GarageFinish.fillna('NA', inplace=True)   
df.GarageType.fillna('NA', inplace=True)     
df.BsmtExposure.fillna('NA', inplace=True)     
df.BsmtCond.fillna('NA', inplace=True)        
df.BsmtQual.fillna('NA', inplace=True)        
df.BsmtFinType2.fillna('NA', inplace=True)     
df.BsmtFinType1.fillna('NA', inplace=True)     
df.MasVnrType.fillna('None', inplace=True)   
df.Exterior2nd.fillna('None', inplace=True) 
```

We could predict these values based on other similar variable in our data using knn algorithm,for example MSZoning variable.for now we can impute or drop rows having nulls of this variable.


```python
#df.dropna(subset =['MSZoning'],inplace=True)
```


```python
df.Functional.fillna(df.Functional.mode()[0], inplace=True)       
df.Utilities.fillna(df.Utilities.mode()[0], inplace=True)          
df.Exterior1st.fillna(df.Exterior1st.mode()[0], inplace=True)        
df.SaleType.fillna(df.SaleType.mode()[0], inplace=True)                
df.KitchenQual.fillna(df.KitchenQual.mode()[0], inplace=True)        
df.Electrical.fillna(df.Electrical.mode()[0], inplace=True)   
```

For now,we have handled null values,we can always comeback and use different imputation strategies

#### Treating Outliers
>@@@@@@@@@To be performed ...left out as we would try tree based models which would be less prone to outliers@@@@@@@@@

##### Categorical variables encoding
http://pbpython.com/categorical-encoding.html
Use either numeric encoding or one-hot encoding. As a guide, you'll want to use one-hot encodind when there's no inherent order to your category, and numeric otherwise.

From the variable with object dtype we can infer that few are nominal and ordinal,we select these and perform encoding on these.


```python
##selecting few cat features to be encoded.
cols_tobe_encoded= ['Alley','BsmtCond','BsmtExposure', 'BsmtFinType1','BsmtFinType2','BsmtQual', 'ExterCond','ExterQual' ,'FireplaceQu', 
                    'Functional','GarageCond', 'GarageQual', 'HeatingQC','KitchenQual','LandSlope', 'PavedDrive', 'PoolQC', 'Street', 'Utilities' ]
df[cols_tobe_encoded].describe()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Alley</th>
      <th>BsmtCond</th>
      <th>BsmtExposure</th>
      <th>BsmtFinType1</th>
      <th>BsmtFinType2</th>
      <th>BsmtQual</th>
      <th>ExterCond</th>
      <th>ExterQual</th>
      <th>FireplaceQu</th>
      <th>Functional</th>
      <th>GarageCond</th>
      <th>GarageQual</th>
      <th>HeatingQC</th>
      <th>KitchenQual</th>
      <th>LandSlope</th>
      <th>PavedDrive</th>
      <th>PoolQC</th>
      <th>Street</th>
      <th>Utilities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>3</td>
      <td>5</td>
      <td>5</td>
      <td>7</td>
      <td>7</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>6</td>
      <td>7</td>
      <td>6</td>
      <td>6</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NA</td>
      <td>TA</td>
      <td>No</td>
      <td>Unf</td>
      <td>Unf</td>
      <td>TA</td>
      <td>TA</td>
      <td>TA</td>
      <td>NA</td>
      <td>Typ</td>
      <td>TA</td>
      <td>TA</td>
      <td>Ex</td>
      <td>TA</td>
      <td>Gtl</td>
      <td>Y</td>
      <td>NA</td>
      <td>Pave</td>
      <td>AllPub</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2721</td>
      <td>2606</td>
      <td>1904</td>
      <td>851</td>
      <td>2493</td>
      <td>1283</td>
      <td>2538</td>
      <td>1798</td>
      <td>1420</td>
      <td>2719</td>
      <td>2654</td>
      <td>2604</td>
      <td>1493</td>
      <td>1493</td>
      <td>2778</td>
      <td>2641</td>
      <td>2909</td>
      <td>2907</td>
      <td>2918</td>
    </tr>
  </tbody>
</table>
</div>




```python
##categorical features having inherent ordering
ordinal_cat_cols= ['BsmtCond','BsmtExposure', 'BsmtFinType1','BsmtFinType2','BsmtQual', 'ExterCond','ExterQual' ,'FireplaceQu', 
                    'Functional','GarageCond', 'GarageQual', 'HeatingQC','KitchenQual', 'PoolQC',]
```


```python
##categorical features which are nominal
nominal_cat_cols= [i for i in cols_tobe_encoded if i not in ordinal_cat_cols]
print(nominal_cat_cols)
```

    ['Alley', 'LandSlope', 'PavedDrive', 'Street', 'Utilities']
    


```python
[i for i in categorical if i not in cols_tobe_encoded]
```




    ['BldgType',
     'CentralAir',
     'Condition1',
     'Condition2',
     'Electrical',
     'Exterior1st',
     'Exterior2nd',
     'Fence',
     'Foundation',
     'GarageFinish',
     'GarageType',
     'Heating',
     'HouseStyle',
     'LandContour',
     'LotConfig',
     'LotShape',
     'MSZoning',
     'MasVnrType',
     'MiscFeature',
     'Neighborhood',
     'RoofMatl',
     'RoofStyle',
     'SaleCondition',
     'SaleType']



>dummy variables tend to increase the multicollinearity in our dataset,therefore it is important to check VIF later and remove high multicollinear variable
http://www.algosome.com/articles/dummy-variable-trap-regression.html


```python
df['BsmtCond']=df['BsmtCond'].astype('category',
                             categories=['NA','Po','Fa','TA','Gd','Ex'],
                             ordered=True)

```


```python
#######
df['ExterQual']=df['ExterQual'].astype('category',
                             categories=['Po','Fa','TA','Gd','Ex'],
                             ordered=True)
#######
df['KitchenQual']=df['KitchenQual'].astype('category',
                             categories=['Po','Fa','TA','Gd','Ex'],
                             ordered=True)

#######
df['PoolQC']=df['PoolQC'].astype('category',
                             categories=['NA','Po','Fa','TA','Gd','Ex'],
                             ordered=True)

```


```python
df['BsmtExposure']=df['BsmtExposure'].astype('category',
                             categories=['NA','No','Mn','Av','Gd'],
                             ordered=True)

df['BsmtFinType1']=df['BsmtFinType1'].astype('category',
                             categories=['NA','Unf', 'LwQ', 'Rec', 'BLQ', 'ALQ', 'GLQ'],
                             ordered=True)

df['BsmtFinType2']=df['BsmtFinType2'].astype('category',
                             categories=['NA','Unf', 'LwQ', 'Rec', 'BLQ', 'ALQ', 'GLQ'],
                             ordered=True)

df['BsmtQual']=df['BsmtQual'].astype('category',
                             categories=['NA','Po','Fa','TA','Gd','Ex'],
                             ordered=True)
df['ExterCond']=df['ExterCond'].astype('category',
                             categories=['Po','Fa','TA','Gd','Ex'],
                                       ordered=True)


df['FireplaceQu']=df['FireplaceQu'].astype('category',
                             categories=['NA','Po','Fa','TA','Gd','Ex'],
                             ordered=True)
df['Functional']=df['Functional'].astype('category',
                             categories=['Sal', 'Sev', 'Maj2', 'Maj1', 'Mod', 'Min2', 'Min1', 'Typ'],
                             ordered=True)

df['GarageQual']=df['GarageQual'].astype('category',
                             categories=['NA','Po','Fa','TA','Gd','Ex'],
                             ordered=True)

df['GarageCond']=df['GarageCond'].astype('category',
                             categories=['NA','Po','Fa','TA','Gd','Ex'],
                             ordered=True)

df['HeatingQC']=df['HeatingQC'].astype('category',
                             categories=['Po','Fa','TA','Gd','Ex'],
                             ordered=True)



```


```python
variable_dtype_plot(df)
```


![png](/regression_housing_prices_files/output_46_0.png)



```python
ordinal_cat_cols= ['BsmtCond','BsmtExposure', 'BsmtFinType1','BsmtFinType2','BsmtQual', 'ExterCond','ExterQual' ,'FireplaceQu', 
                    'Functional','GarageCond', 'GarageQual', 'HeatingQC','KitchenQual', 'PoolQC',]

for col in ordinal_cat_cols:
    df[col] = df[col].cat.codes
    

    

```


```python
##update list of numerical features and categorical features
numerical = numerical + ordinal_cat_cols

for col in ordinal_cat_cols:
    categorical.remove(col)
```

Now we have marked the ordinal columns which are encoded as nos to be numerical features and removed them from categorical features list.

Now the columns remaining in categorical features list would need to be one-hot encoded as there is no inherent ordering in the column values.


```python
df[categorical].describe().T
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>unique</th>
      <th>top</th>
      <th>freq</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alley</th>
      <td>2919</td>
      <td>3</td>
      <td>NA</td>
      <td>2721</td>
    </tr>
    <tr>
      <th>BldgType</th>
      <td>2919</td>
      <td>5</td>
      <td>1Fam</td>
      <td>2425</td>
    </tr>
    <tr>
      <th>CentralAir</th>
      <td>2919</td>
      <td>2</td>
      <td>Y</td>
      <td>2723</td>
    </tr>
    <tr>
      <th>Condition1</th>
      <td>2919</td>
      <td>9</td>
      <td>Norm</td>
      <td>2511</td>
    </tr>
    <tr>
      <th>Condition2</th>
      <td>2919</td>
      <td>8</td>
      <td>Norm</td>
      <td>2889</td>
    </tr>
    <tr>
      <th>Electrical</th>
      <td>2919</td>
      <td>5</td>
      <td>SBrkr</td>
      <td>2672</td>
    </tr>
    <tr>
      <th>Exterior1st</th>
      <td>2919</td>
      <td>15</td>
      <td>VinylSd</td>
      <td>1026</td>
    </tr>
    <tr>
      <th>Exterior2nd</th>
      <td>2919</td>
      <td>17</td>
      <td>VinylSd</td>
      <td>1014</td>
    </tr>
    <tr>
      <th>Fence</th>
      <td>2919</td>
      <td>5</td>
      <td>NA</td>
      <td>2348</td>
    </tr>
    <tr>
      <th>Foundation</th>
      <td>2919</td>
      <td>6</td>
      <td>PConc</td>
      <td>1308</td>
    </tr>
    <tr>
      <th>GarageFinish</th>
      <td>2919</td>
      <td>4</td>
      <td>Unf</td>
      <td>1230</td>
    </tr>
    <tr>
      <th>GarageType</th>
      <td>2919</td>
      <td>7</td>
      <td>Attchd</td>
      <td>1723</td>
    </tr>
    <tr>
      <th>Heating</th>
      <td>2919</td>
      <td>6</td>
      <td>GasA</td>
      <td>2874</td>
    </tr>
    <tr>
      <th>HouseStyle</th>
      <td>2919</td>
      <td>8</td>
      <td>1Story</td>
      <td>1471</td>
    </tr>
    <tr>
      <th>LandContour</th>
      <td>2919</td>
      <td>4</td>
      <td>Lvl</td>
      <td>2622</td>
    </tr>
    <tr>
      <th>LandSlope</th>
      <td>2919</td>
      <td>3</td>
      <td>Gtl</td>
      <td>2778</td>
    </tr>
    <tr>
      <th>LotConfig</th>
      <td>2919</td>
      <td>5</td>
      <td>Inside</td>
      <td>2133</td>
    </tr>
    <tr>
      <th>LotShape</th>
      <td>2919</td>
      <td>4</td>
      <td>Reg</td>
      <td>1859</td>
    </tr>
    <tr>
      <th>MSZoning</th>
      <td>2915</td>
      <td>5</td>
      <td>RL</td>
      <td>2265</td>
    </tr>
    <tr>
      <th>MasVnrType</th>
      <td>2919</td>
      <td>4</td>
      <td>None</td>
      <td>1766</td>
    </tr>
    <tr>
      <th>MiscFeature</th>
      <td>2919</td>
      <td>5</td>
      <td>NA</td>
      <td>2814</td>
    </tr>
    <tr>
      <th>Neighborhood</th>
      <td>2919</td>
      <td>25</td>
      <td>NAmes</td>
      <td>443</td>
    </tr>
    <tr>
      <th>PavedDrive</th>
      <td>2919</td>
      <td>3</td>
      <td>Y</td>
      <td>2641</td>
    </tr>
    <tr>
      <th>RoofMatl</th>
      <td>2919</td>
      <td>8</td>
      <td>CompShg</td>
      <td>2876</td>
    </tr>
    <tr>
      <th>RoofStyle</th>
      <td>2919</td>
      <td>6</td>
      <td>Gable</td>
      <td>2310</td>
    </tr>
    <tr>
      <th>SaleCondition</th>
      <td>2919</td>
      <td>6</td>
      <td>Normal</td>
      <td>2402</td>
    </tr>
    <tr>
      <th>SaleType</th>
      <td>2919</td>
      <td>9</td>
      <td>WD</td>
      <td>2526</td>
    </tr>
    <tr>
      <th>Street</th>
      <td>2919</td>
      <td>2</td>
      <td>Pave</td>
      <td>2907</td>
    </tr>
    <tr>
      <th>Utilities</th>
      <td>2919</td>
      <td>2</td>
      <td>AllPub</td>
      <td>2918</td>
    </tr>
  </tbody>
</table>
</div>




```python
##these variables tend to have no inherent ordering in their values
#df = pd.get_dummies(df,columns=['Alley','LandSlope','PavedDrive','Street','Utilities'])

```

There are still a few non-numerical data in our dataframe which needs to be encoded using one of the techniques.


```python
##MSSubClass is a category feature and needs to be encoded as categories.
df.replace({'MSSubClass':{20:'class1', 30:'class2', 40:'class3', 45:'class4',
                                   50:'class5', 60:'class6', 70:'class7', 75:'class8',
                                   80:'class9', 85:'class10', 90:'class11', 120:'class12',
                                   150:'class13', 160:'class14', 180:'class15', 190:'class16'}},inplace=True)

df['MSSubClass'] = df['MSSubClass'].astype('category')
##update categorical features

categorical.append('MSSubClass')
```


```python
numerical.remove('MSSubClass')
```


```python
df[categorical].describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Alley</th>
      <th>BldgType</th>
      <th>CentralAir</th>
      <th>Condition1</th>
      <th>Condition2</th>
      <th>Electrical</th>
      <th>Exterior1st</th>
      <th>Exterior2nd</th>
      <th>Fence</th>
      <th>Foundation</th>
      <th>...</th>
      <th>MiscFeature</th>
      <th>Neighborhood</th>
      <th>PavedDrive</th>
      <th>RoofMatl</th>
      <th>RoofStyle</th>
      <th>SaleCondition</th>
      <th>SaleType</th>
      <th>Street</th>
      <th>Utilities</th>
      <th>MSSubClass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>...</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
      <td>2919</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>3</td>
      <td>5</td>
      <td>2</td>
      <td>9</td>
      <td>8</td>
      <td>5</td>
      <td>15</td>
      <td>17</td>
      <td>5</td>
      <td>6</td>
      <td>...</td>
      <td>5</td>
      <td>25</td>
      <td>3</td>
      <td>8</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>2</td>
      <td>2</td>
      <td>16</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NA</td>
      <td>1Fam</td>
      <td>Y</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>SBrkr</td>
      <td>VinylSd</td>
      <td>VinylSd</td>
      <td>NA</td>
      <td>PConc</td>
      <td>...</td>
      <td>NA</td>
      <td>NAmes</td>
      <td>Y</td>
      <td>CompShg</td>
      <td>Gable</td>
      <td>Normal</td>
      <td>WD</td>
      <td>Pave</td>
      <td>AllPub</td>
      <td>class1</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2721</td>
      <td>2425</td>
      <td>2723</td>
      <td>2511</td>
      <td>2889</td>
      <td>2672</td>
      <td>1026</td>
      <td>1014</td>
      <td>2348</td>
      <td>1308</td>
      <td>...</td>
      <td>2814</td>
      <td>443</td>
      <td>2641</td>
      <td>2876</td>
      <td>2310</td>
      <td>2402</td>
      <td>2526</td>
      <td>2907</td>
      <td>2918</td>
      <td>1079</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 30 columns</p>
</div>



As we can see,there are many categories in Neigborhood attribute(25),we will analyze this attribute and combine levels of the neighborhood variable into fewer levels.

We plot below the median of saleprice wrt to each neighborhood type and it gives us an idea to combine levels with similar height into a single level.


```python
train_df.groupby('Neighborhood')['SalePrice'].agg({'median':np.median}).sort_values(by='median').plot(kind='bar')
```

    C:\Users\Nithin\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: FutureWarning: using a dict on a Series for aggregation
    is deprecated and will be removed in a future version
      """Entry point for launching an IPython kernel.
    




    <matplotlib.axes._subplots.AxesSubplot at 0x2109188f860>




![png](/regression_housing_prices_files/output_57_2.png)



```python
neighborhood_map = {"MeadowV" : 0, "IDOTRR" : 1, "BrDale" : 1, "OldTown" : 1, "Edwards" : 1,
                    "BrkSide" : 1,"Sawyer" : 1, "Blueste" : 1, "SWISU" : 2, "NAmes" : 2,
                    "NPkVill" : 2, "Mitchel" : 2, "SawyerW" : 2, "Gilbert" : 2, "NWAmes" : 2,
                    "Blmngtn" : 2, "CollgCr" : 2, "ClearCr" : 3, "Crawfor" : 3, "Veenker" : 3,
                    "Somerst" : 3, "Timber" : 3, "StoneBr" : 4, "NoRidge" : 4, "NridgHt" : 4}

df.replace({'Neighborhood':neighborhood_map},inplace=True)

df['Neighborhood'] = df['Neighborhood'].astype('category')

```


```python
train_df.groupby('Exterior1st')['SalePrice'].agg({'median':np.median}).sort_values(by='median').plot(kind='bar')
```

    C:\Users\Nithin\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: FutureWarning: using a dict on a Series for aggregation
    is deprecated and will be removed in a future version
      """Entry point for launching an IPython kernel.
    




    <matplotlib.axes._subplots.AxesSubplot at 0x210916a9a58>




![png](/regression_housing_prices_files/output_59_2.png)



```python
exterior1st_map = {"BrkComm":1,"AsphShn":1,"CBlock":1,"AsbShng":1,"WdShing":2,
                  "Wd Sdng":2,"MetalSd":2,"Stucco":2,"HdBoard":2,"BrkFace":3,"Plywood":3,
                  "VinylSd":4,"CemntBd":4,"Stone":4,"ImStucc":4,}

df.replace({'Exterior1st':exterior1st_map},inplace=True)

df['Exterior1st'] = df['Exterior1st'].astype('category')

```


```python
train_df.groupby('Exterior2nd')['SalePrice'].agg({'median':np.median}).sort_values(by='median').plot(kind='bar')
```

    C:\Users\Nithin\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: FutureWarning: using a dict on a Series for aggregation
    is deprecated and will be removed in a future version
      """Entry point for launching an IPython kernel.
    




    <matplotlib.axes._subplots.AxesSubplot at 0x210917dd0f0>




![png](/regression_housing_prices_files/output_61_2.png)



```python
exterior2nd_map = {'VinylSd':3, 'MetalSd':2, 'Wd Shng':2, 'HdBoard':2, 'Plywood':2, 'Wd Sdng':2,
       'CmentBd':4, 'BrkFace':2, 'Stucco':2, 'AsbShng':1, 'Brk Cmn':2, 'ImStucc':3,
       'AsphShn':2, 'Stone':2, 'Other':4, 'CBlock':1, 'None':0}

df.replace({'Exterior2nd':exterior2nd_map},inplace=True)

df['Exterior2nd'] = df['Exterior2nd'].astype('category')

```


```python
df[categorical].describe().T
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>unique</th>
      <th>top</th>
      <th>freq</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alley</th>
      <td>2919</td>
      <td>3</td>
      <td>NA</td>
      <td>2721</td>
    </tr>
    <tr>
      <th>BldgType</th>
      <td>2919</td>
      <td>5</td>
      <td>1Fam</td>
      <td>2425</td>
    </tr>
    <tr>
      <th>CentralAir</th>
      <td>2919</td>
      <td>2</td>
      <td>Y</td>
      <td>2723</td>
    </tr>
    <tr>
      <th>Condition1</th>
      <td>2919</td>
      <td>9</td>
      <td>Norm</td>
      <td>2511</td>
    </tr>
    <tr>
      <th>Condition2</th>
      <td>2919</td>
      <td>8</td>
      <td>Norm</td>
      <td>2889</td>
    </tr>
    <tr>
      <th>Electrical</th>
      <td>2919</td>
      <td>5</td>
      <td>SBrkr</td>
      <td>2672</td>
    </tr>
    <tr>
      <th>Exterior1st</th>
      <td>2919</td>
      <td>4</td>
      <td>2</td>
      <td>1402</td>
    </tr>
    <tr>
      <th>Exterior2nd</th>
      <td>2919</td>
      <td>5</td>
      <td>2</td>
      <td>1721</td>
    </tr>
    <tr>
      <th>Fence</th>
      <td>2919</td>
      <td>5</td>
      <td>NA</td>
      <td>2348</td>
    </tr>
    <tr>
      <th>Foundation</th>
      <td>2919</td>
      <td>6</td>
      <td>PConc</td>
      <td>1308</td>
    </tr>
    <tr>
      <th>GarageFinish</th>
      <td>2919</td>
      <td>4</td>
      <td>Unf</td>
      <td>1230</td>
    </tr>
    <tr>
      <th>GarageType</th>
      <td>2919</td>
      <td>7</td>
      <td>Attchd</td>
      <td>1723</td>
    </tr>
    <tr>
      <th>Heating</th>
      <td>2919</td>
      <td>6</td>
      <td>GasA</td>
      <td>2874</td>
    </tr>
    <tr>
      <th>HouseStyle</th>
      <td>2919</td>
      <td>8</td>
      <td>1Story</td>
      <td>1471</td>
    </tr>
    <tr>
      <th>LandContour</th>
      <td>2919</td>
      <td>4</td>
      <td>Lvl</td>
      <td>2622</td>
    </tr>
    <tr>
      <th>LandSlope</th>
      <td>2919</td>
      <td>3</td>
      <td>Gtl</td>
      <td>2778</td>
    </tr>
    <tr>
      <th>LotConfig</th>
      <td>2919</td>
      <td>5</td>
      <td>Inside</td>
      <td>2133</td>
    </tr>
    <tr>
      <th>LotShape</th>
      <td>2919</td>
      <td>4</td>
      <td>Reg</td>
      <td>1859</td>
    </tr>
    <tr>
      <th>MSZoning</th>
      <td>2915</td>
      <td>5</td>
      <td>RL</td>
      <td>2265</td>
    </tr>
    <tr>
      <th>MasVnrType</th>
      <td>2919</td>
      <td>4</td>
      <td>None</td>
      <td>1766</td>
    </tr>
    <tr>
      <th>MiscFeature</th>
      <td>2919</td>
      <td>5</td>
      <td>NA</td>
      <td>2814</td>
    </tr>
    <tr>
      <th>Neighborhood</th>
      <td>2919</td>
      <td>5</td>
      <td>2</td>
      <td>1344</td>
    </tr>
    <tr>
      <th>PavedDrive</th>
      <td>2919</td>
      <td>3</td>
      <td>Y</td>
      <td>2641</td>
    </tr>
    <tr>
      <th>RoofMatl</th>
      <td>2919</td>
      <td>8</td>
      <td>CompShg</td>
      <td>2876</td>
    </tr>
    <tr>
      <th>RoofStyle</th>
      <td>2919</td>
      <td>6</td>
      <td>Gable</td>
      <td>2310</td>
    </tr>
    <tr>
      <th>SaleCondition</th>
      <td>2919</td>
      <td>6</td>
      <td>Normal</td>
      <td>2402</td>
    </tr>
    <tr>
      <th>SaleType</th>
      <td>2919</td>
      <td>9</td>
      <td>WD</td>
      <td>2526</td>
    </tr>
    <tr>
      <th>Street</th>
      <td>2919</td>
      <td>2</td>
      <td>Pave</td>
      <td>2907</td>
    </tr>
    <tr>
      <th>Utilities</th>
      <td>2919</td>
      <td>2</td>
      <td>AllPub</td>
      <td>2918</td>
    </tr>
    <tr>
      <th>MSSubClass</th>
      <td>2919</td>
      <td>16</td>
      <td>class1</td>
      <td>1079</td>
    </tr>
  </tbody>
</table>
</div>




```python
###we will drop the month sold as it does not impact the saleprice
df.drop('MoSold',axis=1,inplace=True)
#numerical.remove('MoSold')
```


```python
numerical.remove('MoSold')
sns.boxplot(x='MoSold',y='SalePrice', data=train_df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x21092f44ac8>




![png](/regression_housing_prices_files/output_65_1.png)



```python
#df = pd.get_dummies(df,columns=categorical,drop_first=True)
```


```python
###now we have 213 columns due to one-hot encoding
df.shape
```




    (2919, 80)




```python
###now we plot the distribution of numerical variables in our dataset
#num = [f for f in df.columns if df.dtypes[f] != 'object']
numdf=pd.melt(df,value_vars=numerical)
numgrid=sns.FacetGrid(numdf,col='variable',col_wrap=4,sharex=False,sharey=False)
numgrid=numgrid.map(sns.distplot,'value')
numgrid
```




    <seaborn.axisgrid.FacetGrid at 0x210935ac588>




![png](/regression_housing_prices_files/output_68_1.png)


We see that the target variable SalePrice has a right-skewed distribution. We'll need to log transform this variable so that it becomes normally distributed. A normally distributed (or close to normal) target variable helps in better modeling the relationship between target and independent variables. In addition, linear algorithms assume constant variance in the error term. Alternatively, we can also confirm this skewed behavior using the skewness metric.
,final result is evaluated using the Root-Mean-Squared-Error (RMSE) between the logarithm of the predicted value and the logarithm of the observed sales price,therefore it would be better to log-transform this variable


```python
print ("The skewness of SalePrice is {}".format(df['SalePrice'].skew()))
```

    The skewness of SalePrice is 1.8828757597682129
    


```python
target = np.log(df['SalePrice'].dropna())
print ('Skewness is', target.skew())
sns.distplot(target)
```

    Skewness is 0.121335062205
    




    <matplotlib.axes._subplots.AxesSubplot at 0x21099fb4240>




![png](/regression_housing_prices_files/output_71_2.png)



```python
df['SalePrice'] = np.log(df['SalePrice'])
```


```python
#df['SalePrice']
```


```python
#numdf1 = pd.melt(train_df, id_vars=['SalePrice'],value_vars=numerical)
#numgrid1 = sns.FacetGrid(numdf1, col="variable",  col_wrap=4 , size=3.0,aspect=1.2,sharex=False, sharey=False)
#numgrid1.map(plt.scatter, "value",'SalePrice',s=1.5)
#numgrid1
```


```python
from scipy.stats import skew
skewed = df[numerical].apply(lambda x: skew(x.dropna().astype(float)))
skewed = skewed[skewed>0.75].index
```


```python
skewed
```




    Index(['1stFlrSF', '2ndFlrSF', '3SsnPorch', 'BsmtFinSF1', 'BsmtFinSF2',
           'BsmtHalfBath', 'BsmtUnfSF', 'EnclosedPorch', 'GrLivArea',
           'KitchenAbvGr', 'LotArea', 'LotFrontage', 'LowQualFinSF', 'MasVnrArea',
           'MiscVal', 'OpenPorchSF', 'PoolArea', 'ScreenPorch', 'TotRmsAbvGrd',
           'TotalBsmtSF', 'WoodDeckSF', 'BsmtExposure', 'BsmtFinType2',
           'ExterCond', 'ExterQual', 'PoolQC'],
          dtype='object')




```python
df[skewed]=np.log1p(df[skewed])
```


```python
##we will plot the distributions again to check skewness
numdf=pd.melt(df,value_vars=numerical)
numgrid=sns.FacetGrid(numdf,col='variable',col_wrap=4,sharex=False,sharey=False)
numgrid=numgrid.map(sns.distplot,'value')
numgrid
```




    <seaborn.axisgrid.FacetGrid at 0x21097addf98>




![png](/regression_housing_prices_files/output_78_1.png)


As we have already encoded ordinal categorical variables we are left with encoding the nominal categorical variables using one-hot encoding


```python
df = pd.get_dummies(df,columns=categorical,drop_first=True)
```


```python
df.shape
```




    (2919, 184)



Now we have numerically encoded all our features as seen below in the plot of dtypes.We can now split the train and test data.


```python
variable_dtype_plot(df)
```


![png](/regression_housing_prices_files/output_83_0.png)



```python
X_train  = df[:1460].drop(['SalePrice','Id'], axis=1)
y_train  = df[:1460]['SalePrice']
X_test  = df[1460:].drop(['SalePrice','Id'], axis=1)
```

Now, we'll standardize the numeric features.(those which are not dummy variables).Ensure to fit using train data and transform both train and test data


```python
numerical.remove('SalePrice')
numerical.remove('Id')
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
scaler.fit(X_train[numerical])
X_train[numerical]= scaler.transform(X_train[numerical])
X_test[numerical]= scaler.transform(X_test[numerical])
```


```python
X_train.shape,y_train.shape,X_test.shape
```




    ((1460, 182), (1460,), (1459, 182))



##### Feature Selection
Decision trees are by nature immune to multi-collinearity. For example, if you have 2 features which are 99% correlated, when deciding upon a split the tree will choose only one of them. Other models such as Logistic regression would use both the features.

Since boosted trees use individual decision trees, they also are unaffected by multi-collinearity. However, its a good practice to remove any redundant features from any dataset used for training, irrespective of the model's algorithm. In your case since you're deriving new features, you could use this approach, evaluate each feature's importance and retain only the best features for your final model.

Now there are 221 features due to a large amount of the dummy variable. Overfitting can easily occur when there are redundant features. Therefore I use XGBoost regressor to generate the rank of "feature importance"




```python
import xgboost as xgb
from sklearn.model_selection import GridSearchCV

```

Parameters max_depth and min_child_weight
Those parameters add constraints on the architecture of the trees.

i)max_depth is the maximum number of nodes allowed from the root to the farthest leaf of a tree. Deeper trees can model more complex relationships by adding more nodes, but as we go deeper, splits become less relevant and are sometimes only due to noise, causing the model to overfit.  

ii)min_child_weight is the minimum weight (or number of samples if all samples have a weight of 1) required in order to create a new node in the tree. A smaller min_child_weight allows the algorithm to create children that correspond to fewer samples, thus allowing for more complex trees, but again, more likely to overfit.
Thus, those parameters can be used to control the complexity of the trees.   

It is important to tune them together in order to find a good trade-off between model bias and variance


```python
param_grid = {'max_depth': [3,5,7], 'min_child_weight': [1,3,5]}
xgb_param = {'learning_rate': 0.1, 'n_estimators': 1000, 'seed':0, 'subsample': 0.8, 'colsample_bytree': 0.8, 
             'objective': 'reg:linear'}
xgb_reg = xgb.XGBRegressor(**xgb_param)
optimize_xgb = GridSearchCV(estimator=xgb_reg,param_grid=param_grid,scoring='neg_mean_squared_error',cv = 5, n_jobs = -1)
```


```python
optimize_xgb.fit(X_train,y_train)
```




    GridSearchCV(cv=5, error_score='raise',
           estimator=XGBRegressor(base_score=0.5, colsample_bylevel=1, colsample_bytree=0.8,
           gamma=0, learning_rate=0.1, max_delta_step=0, max_depth=3,
           min_child_weight=1, missing=None, n_estimators=1000, nthread=-1,
           objective='reg:linear', reg_alpha=0, reg_lambda=1,
           scale_pos_weight=1, seed=0, silent=True, subsample=0.8),
           fit_params=None, iid=True, n_jobs=-1,
           param_grid={'max_depth': [3, 5, 7], 'min_child_weight': [1, 3, 5]},
           pre_dispatch='2*n_jobs', refit=True, return_train_score=True,
           scoring='neg_mean_squared_error', verbose=0)




```python
optimize_xgb.grid_scores_
```

    C:\Users\Nithin\Anaconda3\lib\site-packages\sklearn\model_selection\_search.py:747: DeprecationWarning: The grid_scores_ attribute was deprecated in version 0.18 in favor of the more elaborate cv_results_ attribute. The grid_scores_ attribute will not be available from 0.20
      DeprecationWarning)
    




    [mean: -0.01533, std: 0.00226, params: {'max_depth': 3, 'min_child_weight': 1},
     mean: -0.01583, std: 0.00167, params: {'max_depth': 3, 'min_child_weight': 3},
     mean: -0.01605, std: 0.00176, params: {'max_depth': 3, 'min_child_weight': 5},
     mean: -0.01640, std: 0.00197, params: {'max_depth': 5, 'min_child_weight': 1},
     mean: -0.01618, std: 0.00136, params: {'max_depth': 5, 'min_child_weight': 3},
     mean: -0.01624, std: 0.00166, params: {'max_depth': 5, 'min_child_weight': 5},
     mean: -0.01702, std: 0.00210, params: {'max_depth': 7, 'min_child_weight': 1},
     mean: -0.01641, std: 0.00230, params: {'max_depth': 7, 'min_child_weight': 3},
     mean: -0.01603, std: 0.00190, params: {'max_depth': 7, 'min_child_weight': 5}]



we could tune more hyperparameters but for now we will use these values due to compute resources shortage for tuning large sets of hyperparameters.
Parameters num_boost_round and early_stopping_rounds will be helpful in improving accuracy.
This parameter is called num_boost_round and corresponds to the number of boosting rounds or trees to build. Its optimal value highly depends on the other parameters, and thus it should be re-tuned each time you update a parameter. You could do this by tuning it together with all parameters in a grid-search, but it requires a lot of computational effort.

Fortunately XGBoost provides a nice way to find the best number of rounds whilst training. Since trees are built sequentially, instead of fixing the number of rounds at the beginning, we can test our model at each step and see if adding a new tree/round improves performance.

To do so, we define a test dataset and a metric that is used to assess performance at each round. If performance haven't improved for N rounds (N is defined by the variable early_stopping_round), we stop the training and keep the best number of boosting rounds.

For this we will be using XGBoost API's cv method.

https://cambridgespark.com/content/tutorials/hyperparameter-tuning-in-xgboost/index.html


```python
xgdmat = xgb.DMatrix(X_train, y_train) # Create our DMatrix to make XGBoost more efficient
```


```python
##from above cross-validation GridSearch
our_params = {'eta': 0.1, 'seed':0, 'subsample': 0.8, 'colsample_bytree': 0.8, 
             'objective': 'reg:linear', 'max_depth':3, 'min_child_weight':1} 
cv_xgb = xgb.cv(params = our_params, dtrain = xgdmat, num_boost_round = 3000, nfold = 5,
                metrics = ['rmse'], # Make sure you enter metrics inside a list or you may encounter issues!
                early_stopping_rounds = 100)
```

cv_xgb is a dataframe where the rows correspond to the number of boosting trees used, here again, we stopped before the 3000 rounds


```python
cv_xgb
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>test-rmse-mean</th>
      <th>test-rmse-std</th>
      <th>train-rmse-mean</th>
      <th>train-rmse-std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.379822</td>
      <td>0.033839</td>
      <td>10.379870</td>
      <td>0.007184</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.344127</td>
      <td>0.034248</td>
      <td>9.344181</td>
      <td>0.006840</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.412292</td>
      <td>0.035014</td>
      <td>8.412352</td>
      <td>0.006178</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7.574085</td>
      <td>0.035852</td>
      <td>7.574152</td>
      <td>0.005454</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6.819066</td>
      <td>0.035495</td>
      <td>6.819701</td>
      <td>0.004287</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6.140290</td>
      <td>0.032772</td>
      <td>6.140391</td>
      <td>0.003767</td>
    </tr>
    <tr>
      <th>6</th>
      <td>5.530189</td>
      <td>0.030912</td>
      <td>5.529307</td>
      <td>0.003420</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4.979819</td>
      <td>0.029225</td>
      <td>4.979449</td>
      <td>0.003466</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4.484459</td>
      <td>0.027517</td>
      <td>4.483990</td>
      <td>0.003051</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4.039501</td>
      <td>0.026085</td>
      <td>4.038489</td>
      <td>0.002739</td>
    </tr>
    <tr>
      <th>10</th>
      <td>3.638263</td>
      <td>0.025406</td>
      <td>3.637138</td>
      <td>0.002289</td>
    </tr>
    <tr>
      <th>11</th>
      <td>3.277578</td>
      <td>0.025044</td>
      <td>3.276346</td>
      <td>0.002096</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2.952482</td>
      <td>0.024389</td>
      <td>2.951513</td>
      <td>0.002167</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2.659741</td>
      <td>0.023205</td>
      <td>2.659352</td>
      <td>0.001912</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2.396945</td>
      <td>0.022857</td>
      <td>2.396294</td>
      <td>0.001507</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2.160222</td>
      <td>0.021756</td>
      <td>2.159610</td>
      <td>0.001576</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1.947135</td>
      <td>0.020532</td>
      <td>1.946340</td>
      <td>0.001429</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1.755617</td>
      <td>0.020124</td>
      <td>1.754581</td>
      <td>0.001241</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1.582870</td>
      <td>0.018943</td>
      <td>1.581904</td>
      <td>0.001236</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1.427049</td>
      <td>0.018012</td>
      <td>1.426389</td>
      <td>0.001120</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1.287859</td>
      <td>0.017427</td>
      <td>1.286661</td>
      <td>0.000922</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1.162530</td>
      <td>0.016839</td>
      <td>1.161176</td>
      <td>0.000801</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1.049724</td>
      <td>0.016123</td>
      <td>1.048125</td>
      <td>0.000848</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.948474</td>
      <td>0.015700</td>
      <td>0.946466</td>
      <td>0.000842</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.856856</td>
      <td>0.015552</td>
      <td>0.854900</td>
      <td>0.000680</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.774883</td>
      <td>0.014861</td>
      <td>0.772707</td>
      <td>0.000879</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0.700937</td>
      <td>0.014548</td>
      <td>0.698810</td>
      <td>0.001033</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0.635141</td>
      <td>0.014218</td>
      <td>0.632490</td>
      <td>0.000780</td>
    </tr>
    <tr>
      <th>28</th>
      <td>0.576207</td>
      <td>0.013976</td>
      <td>0.572851</td>
      <td>0.000707</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.523479</td>
      <td>0.013901</td>
      <td>0.519411</td>
      <td>0.000751</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>254</th>
      <td>0.122656</td>
      <td>0.023641</td>
      <td>0.058347</td>
      <td>0.002054</td>
    </tr>
    <tr>
      <th>255</th>
      <td>0.122669</td>
      <td>0.023616</td>
      <td>0.058241</td>
      <td>0.002025</td>
    </tr>
    <tr>
      <th>256</th>
      <td>0.122685</td>
      <td>0.023640</td>
      <td>0.058115</td>
      <td>0.001961</td>
    </tr>
    <tr>
      <th>257</th>
      <td>0.122685</td>
      <td>0.023619</td>
      <td>0.058017</td>
      <td>0.001935</td>
    </tr>
    <tr>
      <th>258</th>
      <td>0.122629</td>
      <td>0.023698</td>
      <td>0.057908</td>
      <td>0.001909</td>
    </tr>
    <tr>
      <th>259</th>
      <td>0.122571</td>
      <td>0.023667</td>
      <td>0.057779</td>
      <td>0.001914</td>
    </tr>
    <tr>
      <th>260</th>
      <td>0.122605</td>
      <td>0.023673</td>
      <td>0.057654</td>
      <td>0.001936</td>
    </tr>
    <tr>
      <th>261</th>
      <td>0.122616</td>
      <td>0.023668</td>
      <td>0.057507</td>
      <td>0.001962</td>
    </tr>
    <tr>
      <th>262</th>
      <td>0.122573</td>
      <td>0.023633</td>
      <td>0.057365</td>
      <td>0.001951</td>
    </tr>
    <tr>
      <th>263</th>
      <td>0.122546</td>
      <td>0.023652</td>
      <td>0.057179</td>
      <td>0.001928</td>
    </tr>
    <tr>
      <th>264</th>
      <td>0.122535</td>
      <td>0.023687</td>
      <td>0.057031</td>
      <td>0.001918</td>
    </tr>
    <tr>
      <th>265</th>
      <td>0.122499</td>
      <td>0.023673</td>
      <td>0.056924</td>
      <td>0.001915</td>
    </tr>
    <tr>
      <th>266</th>
      <td>0.122504</td>
      <td>0.023701</td>
      <td>0.056810</td>
      <td>0.001923</td>
    </tr>
    <tr>
      <th>267</th>
      <td>0.122500</td>
      <td>0.023755</td>
      <td>0.056659</td>
      <td>0.001932</td>
    </tr>
    <tr>
      <th>268</th>
      <td>0.122542</td>
      <td>0.023772</td>
      <td>0.056524</td>
      <td>0.001897</td>
    </tr>
    <tr>
      <th>269</th>
      <td>0.122571</td>
      <td>0.023703</td>
      <td>0.056375</td>
      <td>0.001889</td>
    </tr>
    <tr>
      <th>270</th>
      <td>0.122558</td>
      <td>0.023742</td>
      <td>0.056271</td>
      <td>0.001878</td>
    </tr>
    <tr>
      <th>271</th>
      <td>0.122540</td>
      <td>0.023715</td>
      <td>0.056126</td>
      <td>0.001883</td>
    </tr>
    <tr>
      <th>272</th>
      <td>0.122537</td>
      <td>0.023754</td>
      <td>0.055990</td>
      <td>0.001877</td>
    </tr>
    <tr>
      <th>273</th>
      <td>0.122529</td>
      <td>0.023788</td>
      <td>0.055833</td>
      <td>0.001884</td>
    </tr>
    <tr>
      <th>274</th>
      <td>0.122520</td>
      <td>0.023803</td>
      <td>0.055730</td>
      <td>0.001892</td>
    </tr>
    <tr>
      <th>275</th>
      <td>0.122519</td>
      <td>0.023809</td>
      <td>0.055624</td>
      <td>0.001864</td>
    </tr>
    <tr>
      <th>276</th>
      <td>0.122499</td>
      <td>0.023802</td>
      <td>0.055531</td>
      <td>0.001843</td>
    </tr>
    <tr>
      <th>277</th>
      <td>0.122440</td>
      <td>0.023850</td>
      <td>0.055432</td>
      <td>0.001824</td>
    </tr>
    <tr>
      <th>278</th>
      <td>0.122426</td>
      <td>0.023860</td>
      <td>0.055281</td>
      <td>0.001801</td>
    </tr>
    <tr>
      <th>279</th>
      <td>0.122456</td>
      <td>0.023842</td>
      <td>0.055165</td>
      <td>0.001770</td>
    </tr>
    <tr>
      <th>280</th>
      <td>0.122492</td>
      <td>0.023872</td>
      <td>0.055077</td>
      <td>0.001774</td>
    </tr>
    <tr>
      <th>281</th>
      <td>0.122458</td>
      <td>0.023897</td>
      <td>0.054932</td>
      <td>0.001792</td>
    </tr>
    <tr>
      <th>282</th>
      <td>0.122454</td>
      <td>0.023887</td>
      <td>0.054816</td>
      <td>0.001775</td>
    </tr>
    <tr>
      <th>283</th>
      <td>0.122425</td>
      <td>0.023908</td>
      <td>0.054718</td>
      <td>0.001784</td>
    </tr>
  </tbody>
</table>
<p>284 rows × 4 columns</p>
</div>




```python
cv_xgb['test-rmse-mean'].min()
cv_xgb.loc[30:,["test-rmse-mean", "train-rmse-mean"]].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2109d80c6d8>




![png](/regression_housing_prices_files/output_99_1.png)


As you can see we stopped before reaching the maximum number of boosting rounds, that's because after the 284th tree, adding more rounds did not lead to improvements of RMSE on the test dataset.


```python
#xgb_reg = xgb.XGBRegressor(**our_params)
final_gb = xgb.train(our_params, xgdmat, num_boost_round = 284)

```


```python
%matplotlib inline
import seaborn as sns
sns.set(font_scale = 1.5)
```


```python
sorted(final_gb.get_fscore().items())
```




    [('1stFlrSF', 122),
     ('2ndFlrSF', 39),
     ('3SsnPorch', 5),
     ('Alley_NA', 4),
     ('Alley_Pave', 10),
     ('BedroomAbvGr', 24),
     ('BldgType_2fmCon', 2),
     ('BldgType_Duplex', 1),
     ('BsmtCond', 11),
     ('BsmtExposure', 16),
     ('BsmtFinSF1', 67),
     ('BsmtFinSF2', 17),
     ('BsmtFinType1', 12),
     ('BsmtFinType2', 6),
     ('BsmtFullBath', 19),
     ('BsmtHalfBath', 3),
     ('BsmtQual', 11),
     ('BsmtUnfSF', 77),
     ('CentralAir_Y', 13),
     ('Condition1_Feedr', 3),
     ('Condition1_Norm', 19),
     ('Condition1_PosA', 1),
     ('Condition1_PosN', 3),
     ('Condition1_RRAe', 5),
     ('Condition1_RRAn', 1),
     ('Electrical_FuseF', 2),
     ('Electrical_SBrkr', 4),
     ('EnclosedPorch', 39),
     ('ExterCond', 17),
     ('ExterQual', 3),
     ('Exterior1st_2', 5),
     ('Exterior1st_3', 11),
     ('Exterior1st_4', 2),
     ('Exterior2nd_1', 10),
     ('Exterior2nd_2', 1),
     ('Exterior2nd_3', 1),
     ('Fence_GdWo', 8),
     ('Fence_MnPrv', 2),
     ('Fence_NA', 4),
     ('FireplaceQu', 8),
     ('Fireplaces', 4),
     ('Foundation_CBlock', 1),
     ('Foundation_PConc', 8),
     ('Foundation_Wood', 3),
     ('FullBath', 8),
     ('Functional', 31),
     ('GarageArea', 70),
     ('GarageCars', 8),
     ('GarageCond', 4),
     ('GarageFinish_RFn', 2),
     ('GarageFinish_Unf', 5),
     ('GarageQual', 5),
     ('GarageType_Attchd', 12),
     ('GarageType_Basment', 2),
     ('GarageType_BuiltIn', 1),
     ('GarageType_CarPort', 1),
     ('GarageType_Detchd', 4),
     ('GarageYrBlt', 56),
     ('GrLivArea', 116),
     ('HalfBath', 7),
     ('HeatingQC', 10),
     ('Heating_GasA', 3),
     ('Heating_Grav', 1),
     ('HouseStyle_1Story', 4),
     ('HouseStyle_2.5Unf', 2),
     ('HouseStyle_2Story', 1),
     ('HouseStyle_SLvl', 2),
     ('KitchenAbvGr', 8),
     ('KitchenQual', 20),
     ('LandContour_HLS', 1),
     ('LandContour_Low', 1),
     ('LandContour_Lvl', 5),
     ('LandSlope_Mod', 2),
     ('LandSlope_Sev', 1),
     ('LotArea', 102),
     ('LotConfig_CulDSac', 8),
     ('LotConfig_FR2', 4),
     ('LotConfig_FR3', 1),
     ('LotConfig_Inside', 5),
     ('LotFrontage', 52),
     ('LotShape_Reg', 2),
     ('LowQualFinSF', 7),
     ('MSSubClass_class2', 5),
     ('MSSubClass_class5', 7),
     ('MSSubClass_class6', 4),
     ('MSSubClass_class7', 1),
     ('MSSubClass_class8', 1),
     ('MSSubClass_class9', 1),
     ('MSZoning_FV', 2),
     ('MSZoning_RH', 1),
     ('MSZoning_RL', 5),
     ('MSZoning_RM', 5),
     ('MasVnrArea', 17),
     ('MasVnrType_BrkFace', 3),
     ('MasVnrType_Stone', 1),
     ('MiscVal', 3),
     ('Neighborhood_1', 4),
     ('Neighborhood_2', 7),
     ('Neighborhood_3', 10),
     ('Neighborhood_4', 8),
     ('OpenPorchSF', 38),
     ('OverallCond', 52),
     ('OverallQual', 68),
     ('PavedDrive_P', 1),
     ('PavedDrive_Y', 7),
     ('PoolArea', 8),
     ('RoofMatl_CompShg', 1),
     ('RoofMatl_Tar&Grv', 1),
     ('RoofMatl_WdShngl', 4),
     ('RoofStyle_Gable', 2),
     ('RoofStyle_Gambrel', 7),
     ('RoofStyle_Hip', 2),
     ('SaleCondition_Alloca', 5),
     ('SaleCondition_Family', 13),
     ('SaleCondition_Normal', 23),
     ('SaleCondition_Partial', 9),
     ('SaleType_ConLI', 1),
     ('SaleType_New', 9),
     ('SaleType_WD', 3),
     ('ScreenPorch', 22),
     ('Street_Pave', 9),
     ('TotRmsAbvGrd', 11),
     ('TotalBsmtSF', 62),
     ('WoodDeckSF', 37),
     ('YearBuilt', 65),
     ('YearRemodAdd', 42),
     ('YrSold', 19)]




```python
##xgb.plot_importance(final_gb,max_num_features=50)
fig, ax = plt.subplots(figsize=(12,18))
#xgb.plot_importance(final_gb, max_num_features=50, height=0.8, ax=ax)
#plt.show()
%matplotlib inline
max_x=10
xgb.plot_importance(dict(sorted(final_gb.get_fscore().items(), reverse = True, key=lambda x:x[1])[:max_x]), height = 0.8)

```




    <matplotlib.axes._subplots.AxesSubplot at 0x210fbb01240>




![png](/regression_housing_prices_files/output_104_1.png)


Now that we have an understanding of the feature importances, we can at least figure out better what is driving the splits most for the trees and where we may be able to make some improvements in feature engineering if possible. You can try playing around with the hyperparameters yourself or engineer some new features to see if you can beat the current benchmarks

The model has now been tuned using cross-validation grid search through the sklearn API and early stopping through the built-in XGBoost API. Now, we can see how it finally performs on the test set.


```python
testdmat = xgb.DMatrix(X_test)
y_pred = final_gb.predict(testdmat) # Predict using our testdmat

```


```python
y_pred
```




    array([ 11.73430061,  11.97035789,  12.14089966, ...,  12.00389957,
            11.66779327,  12.29656219], dtype=float32)




```python
y_pred = np.exp(y_pred)

output = pd.DataFrame({'Id': test_df['Id'], 'SalePrice': y_pred})
```


```python
output.to_csv('prediction-ensemble.csv', index=False)

```

As we can see 


```python
#cv_xgb.shape
#print("Best MAE: {:.2f} with {} rounds".format(
#                 model.best_score,
#                 model.best_iteration+1))
```


```python
#from xgboost import XGBRegressor
#xgb = XGBRegressor()
#xgb.fit(X_train, y_train)
#print(xgb.booster().get_score())
#imp = pd.DataFrame(xgb.feature_importances_ ,columns = ['Importance'],index = X_train.columns)
#imp = pd.DataFrame(xgb.booster().get_score(),index = X_train.columns)
#imp = imp.sort_values(['Importance'], ascending = False)
#model.booster().get_score(importance_type='weight')
#print(imp)
```
