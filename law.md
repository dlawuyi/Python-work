# SUPERSTORE RETAIL STORE.

A BUSINESS ANALYSIS FROM A RETAIL STORE WITH THE SOLE AIM OF SETTING A STANDARD HIGH ENOUGH TO COMPETE GLOBALLY.

THE RAW DATA IS GOTTEN FROM THE FISCAL YEAR OF 2014 FROM THEIR RECORD OF SALES GLOBALLY ACROSS FOUR REGIONS WHICH ARE WEST, SOUTH, EAST AND THE CENTRAL REGION.

THIS ANALYIS IS BASED TO CHECK THE PERFORMANCE OF ALL THE FOUR REGIONS IN RESPECT TO SALES AND PROFIT.

AT THE END, WE GIVE A RECOMMENDATION ON WHAT TO BE DONE TO INCREASE SALES IN THE REGION WITH THE LOWEST PERFORMANCE.


```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
data = pd.read_excel (r'documents\data_superstores.xlsx')
print (data)
```

          Row ID  Order ID Order Priority  Discount  Unit Price  Shipping Cost  \
    0      20847     88522           High      0.01        2.84           0.93   
    1      20228     90193  Not Specified      0.02      500.98          26.00   
    2      21776     90192       Critical      0.06        9.48           7.29   
    3      24844     86838         Medium      0.09       78.69          19.99   
    4      24846     86838         Medium      0.08        3.28           2.31   
    ...      ...       ...            ...       ...         ...            ...   
    1947   19842     87536           High      0.01       10.90           7.46   
    1948   19843     87536           High      0.10        7.99           5.03   
    1949   26208     87534  Not Specified      0.08       11.97           5.81   
    1950   24911     87537         Medium      0.10        9.38           4.93   
    1951   25914     87530           High      0.10      105.98          13.99   
    
          Customer ID      Customer Name       Ship Mode Customer Segment  ...  \
    0               3      Bonnie Potter     Express Air        Corporate  ...   
    1               5     Ronnie Proctor  Delivery Truck      Home Office  ...   
    2              11      Marcus Dunlap     Regular Air      Home Office  ...   
    3              14  Gwendolyn F Tyson     Regular Air   Small Business  ...   
    4              14  Gwendolyn F Tyson     Regular Air   Small Business  ...   
    ...           ...                ...             ...              ...  ...   
    1947         3397        Andrea Shaw     Regular Air   Small Business  ...   
    1948         3397        Andrea Shaw     Regular Air   Small Business  ...   
    1949         3399        Marvin Reid     Regular Air   Small Business  ...   
    1950         3400      Florence Gold     Express Air   Small Business  ...   
    1951         3403      Tammy Buckley     Express Air         Consumer  ...   
    
                Country   Region State or Province         City  Postal Code  \
    0     United States     West        Washington    Anacortes        98221   
    1     United States     West        California  San Gabriel        91776   
    2     United States     East        New Jersey      Roselle         7203   
    3     United States  Central         Minnesota   Prior Lake        55372   
    4     United States  Central         Minnesota   Prior Lake        55372   
    ...             ...      ...               ...          ...          ...   
    1947  United States  Central          Illinois     Danville        61832   
    1948  United States  Central          Illinois     Danville        61832   
    1949  United States  Central          Illinois  Des Plaines        60016   
    1950  United States     East     West Virginia     Fairmont        26554   
    1951  United States     West           Wyoming     Cheyenne        82001   
    
         Order Date  Ship Date     Profit Quantity ordered new    Sales  
    0    2015-01-07 2015-01-08     4.5600                    4    13.01  
    1    2015-06-13 2015-06-15  4390.3665                   12  6362.85  
    2    2015-02-15 2015-02-17   -53.8096                   22   211.15  
    3    2015-05-12 2015-05-14   803.4705                   16  1164.45  
    4    2015-05-12 2015-05-13   -24.0300                    7    22.23  
    ...         ...        ...        ...                  ...      ...  
    1947 2015-03-11 2015-03-12  -116.7600                   18   207.31  
    1948 2015-03-11 2015-03-12  -160.9520                   22   143.12  
    1949 2015-03-29 2015-03-31   -41.8700                    5    59.98  
    1950 2015-04-04 2015-04-04   -24.7104                   15   135.78  
    1951 2015-02-08 2015-02-11   349.4850                    5   506.50  
    
    [1952 rows x 25 columns]
    


```python
data.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Row ID</th>
      <th>Order ID</th>
      <th>Order Priority</th>
      <th>Discount</th>
      <th>Unit Price</th>
      <th>Shipping Cost</th>
      <th>Customer ID</th>
      <th>Customer Name</th>
      <th>Ship Mode</th>
      <th>Customer Segment</th>
      <th>...</th>
      <th>Country</th>
      <th>Region</th>
      <th>State or Province</th>
      <th>City</th>
      <th>Postal Code</th>
      <th>Order Date</th>
      <th>Ship Date</th>
      <th>Profit</th>
      <th>Quantity ordered new</th>
      <th>Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20847</td>
      <td>88522</td>
      <td>High</td>
      <td>0.01</td>
      <td>2.84</td>
      <td>0.93</td>
      <td>3</td>
      <td>Bonnie Potter</td>
      <td>Express Air</td>
      <td>Corporate</td>
      <td>...</td>
      <td>United States</td>
      <td>West</td>
      <td>Washington</td>
      <td>Anacortes</td>
      <td>98221</td>
      <td>2015-01-07</td>
      <td>2015-01-08</td>
      <td>4.5600</td>
      <td>4</td>
      <td>13.01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20228</td>
      <td>90193</td>
      <td>Not Specified</td>
      <td>0.02</td>
      <td>500.98</td>
      <td>26.00</td>
      <td>5</td>
      <td>Ronnie Proctor</td>
      <td>Delivery Truck</td>
      <td>Home Office</td>
      <td>...</td>
      <td>United States</td>
      <td>West</td>
      <td>California</td>
      <td>San Gabriel</td>
      <td>91776</td>
      <td>2015-06-13</td>
      <td>2015-06-15</td>
      <td>4390.3665</td>
      <td>12</td>
      <td>6362.85</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21776</td>
      <td>90192</td>
      <td>Critical</td>
      <td>0.06</td>
      <td>9.48</td>
      <td>7.29</td>
      <td>11</td>
      <td>Marcus Dunlap</td>
      <td>Regular Air</td>
      <td>Home Office</td>
      <td>...</td>
      <td>United States</td>
      <td>East</td>
      <td>New Jersey</td>
      <td>Roselle</td>
      <td>7203</td>
      <td>2015-02-15</td>
      <td>2015-02-17</td>
      <td>-53.8096</td>
      <td>22</td>
      <td>211.15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>24844</td>
      <td>86838</td>
      <td>Medium</td>
      <td>0.09</td>
      <td>78.69</td>
      <td>19.99</td>
      <td>14</td>
      <td>Gwendolyn F Tyson</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>Central</td>
      <td>Minnesota</td>
      <td>Prior Lake</td>
      <td>55372</td>
      <td>2015-05-12</td>
      <td>2015-05-14</td>
      <td>803.4705</td>
      <td>16</td>
      <td>1164.45</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24846</td>
      <td>86838</td>
      <td>Medium</td>
      <td>0.08</td>
      <td>3.28</td>
      <td>2.31</td>
      <td>14</td>
      <td>Gwendolyn F Tyson</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>Central</td>
      <td>Minnesota</td>
      <td>Prior Lake</td>
      <td>55372</td>
      <td>2015-05-12</td>
      <td>2015-05-13</td>
      <td>-24.0300</td>
      <td>7</td>
      <td>22.23</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24847</td>
      <td>86838</td>
      <td>Medium</td>
      <td>0.05</td>
      <td>3.28</td>
      <td>4.20</td>
      <td>14</td>
      <td>Gwendolyn F Tyson</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>Central</td>
      <td>Minnesota</td>
      <td>Prior Lake</td>
      <td>55372</td>
      <td>2015-05-12</td>
      <td>2015-05-13</td>
      <td>-37.0300</td>
      <td>4</td>
      <td>13.99</td>
    </tr>
    <tr>
      <th>6</th>
      <td>24848</td>
      <td>86838</td>
      <td>Medium</td>
      <td>0.05</td>
      <td>3.58</td>
      <td>1.63</td>
      <td>14</td>
      <td>Gwendolyn F Tyson</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>Central</td>
      <td>Minnesota</td>
      <td>Prior Lake</td>
      <td>55372</td>
      <td>2015-05-12</td>
      <td>2015-05-13</td>
      <td>-0.7100</td>
      <td>4</td>
      <td>14.26</td>
    </tr>
    <tr>
      <th>7</th>
      <td>18181</td>
      <td>86837</td>
      <td>Critical</td>
      <td>0.00</td>
      <td>4.42</td>
      <td>4.99</td>
      <td>15</td>
      <td>Timothy Reese</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>East</td>
      <td>New York</td>
      <td>Smithtown</td>
      <td>11787</td>
      <td>2015-04-08</td>
      <td>2015-04-09</td>
      <td>-59.8200</td>
      <td>7</td>
      <td>33.47</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20925</td>
      <td>86839</td>
      <td>Medium</td>
      <td>0.01</td>
      <td>35.94</td>
      <td>6.66</td>
      <td>15</td>
      <td>Timothy Reese</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>East</td>
      <td>New York</td>
      <td>Smithtown</td>
      <td>11787</td>
      <td>2015-05-28</td>
      <td>2015-05-28</td>
      <td>261.8757</td>
      <td>10</td>
      <td>379.53</td>
    </tr>
    <tr>
      <th>9</th>
      <td>26267</td>
      <td>86836</td>
      <td>High</td>
      <td>0.04</td>
      <td>2.98</td>
      <td>1.58</td>
      <td>16</td>
      <td>Sarah Ramsey</td>
      <td>Regular Air</td>
      <td>Small Business</td>
      <td>...</td>
      <td>United States</td>
      <td>East</td>
      <td>New York</td>
      <td>Syracuse</td>
      <td>13210</td>
      <td>2015-02-12</td>
      <td>2015-02-15</td>
      <td>2.6300</td>
      <td>6</td>
      <td>18.80</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 25 columns</p>
</div>



FROM THE DATA SET, WE CAN SEE THAT THE DATA SET APPEARS WHOLE WITH NO NULL VALUES


```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1952 entries, 0 to 1951
    Data columns (total 25 columns):
     #   Column                Non-Null Count  Dtype         
    ---  ------                --------------  -----         
     0   Row ID                1952 non-null   int64         
     1   Order ID              1952 non-null   int64         
     2   Order Priority        1952 non-null   object        
     3   Discount              1952 non-null   float64       
     4   Unit Price            1952 non-null   float64       
     5   Shipping Cost         1952 non-null   float64       
     6   Customer ID           1952 non-null   int64         
     7   Customer Name         1952 non-null   object        
     8   Ship Mode             1952 non-null   object        
     9   Customer Segment      1952 non-null   object        
     10  Product Category      1952 non-null   object        
     11  Product Sub-Category  1952 non-null   object        
     12  Product Container     1952 non-null   object        
     13  Product Name          1952 non-null   object        
     14  Product Base Margin   1936 non-null   float64       
     15  Country               1952 non-null   object        
     16  Region                1952 non-null   object        
     17  State or Province     1952 non-null   object        
     18  City                  1952 non-null   object        
     19  Postal Code           1952 non-null   int64         
     20  Order Date            1952 non-null   datetime64[ns]
     21  Ship Date             1952 non-null   datetime64[ns]
     22  Profit                1952 non-null   float64       
     23  Quantity ordered new  1952 non-null   int64         
     24  Sales                 1952 non-null   float64       
    dtypes: datetime64[ns](2), float64(6), int64(5), object(12)
    memory usage: 381.4+ KB
    


```python
THE DATA COUNT PREPARED FROM THE DATA SET THAT WAS PROVIDED, SHOWS THAT THE SOUTHERN REGION HAS LOW PARTRONAGE COMPARED TO THE OTHER REGIONS. 
```


```python
data['Region'].value_counts()
```




    Central    566
    East       474
    West       470
    South      442
    Name: Region, dtype: int64



SOUTH SHOWS THAT IT IS THE REGION WITH THE LOWEST PROFIT WHICH MIGHT AS WELL BE RUNNING AT A LOSS.



```python
sns.barplot(x = 'Region', y = 'Profit', data = data);
```


![png](output_9_0.png)



```python
IN TERMS OF SALES, SOUTH SLIGHTLY INCREASE TO CENTRAL REGION WHICH SHOWS THAT CENTRAL REGION HAS LOW SALES FOLLOWED BY THE SOUTHERN REGION.
```


```python
sns.barplot(x = 'Region', y = 'Sales', data = data);
```


![png](output_11_0.png)



```python
plt.figure(figsize=(14,6))
sns.lineplot(data=data[['Sales' , 'Unit Price']], linewidth=2.5);
```


![png](output_12_0.png)


FROM THE ANALYSES OF SALES TO UNIT PRICE, SALES APPEARS TO BE MORE RAMPANT TOWARDS $1000 WITH A LOWER UNIT PRICE WHICH SHOWS THAT GOODS WITHIN THAT RANGE SELLS MORE FOLLOWED BY GOODS SLIGHTLY ABOVE $250.

GOODS WITH HIGHER UNIT PRICE TENDS NOT TO SELL WELL.




```python
CONCLUSION

1.TO BOOST SALES IN THE SOUTHERN REGION, THE STORE WOULD LIKE TO CONDUCT A PROMO SALES SO HAS TO DRAW MORE CUSTOMERS TO THE STORE.
2. THE STORE CAN LOOK INTO THERE BEST BUYER AND OFFER TO GIVE THEM A DISCOUNT FROM SALES AND ALSO INFORM THEM OF A CERTAIN PERCENTAGE WOULG BE GIVEN TO THEM IF THEY CAN BE ABLE TO REFER NEW CUSTORMERS TO THEM.

```
