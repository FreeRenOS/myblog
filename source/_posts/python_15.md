---
title: temp
date: 2021-02-04 11:19:07
tags:
---

```python
import pandas as pd
import numpy as np
```


```python
name_1880 = pd.read_csv('data/babynames/yob1880.txt', names=['name','gender','births'])
name_1880
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mary</td>
      <td>F</td>
      <td>7065</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anna</td>
      <td>F</td>
      <td>2604</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Emma</td>
      <td>F</td>
      <td>2003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Elizabeth</td>
      <td>F</td>
      <td>1939</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Minnie</td>
      <td>F</td>
      <td>1746</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1995</th>
      <td>Woodie</td>
      <td>M</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>Worthy</td>
      <td>M</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>Wright</td>
      <td>M</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>York</td>
      <td>M</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>Zachariah</td>
      <td>M</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>2000 rows × 3 columns</p>
</div>




```python
name_1880.groupby('gender').births.sum()
```




    gender
    F     90993
    M    110493
    Name: births, dtype: int64




```python
years = range(1880,2011)
pieces = []
columns = ['name','gender','births']

for year in years :
    path = 'data/babynames/yob{}.txt'.format(year)
    frame = pd.read_csv(path, names = columns)
    frame['year']=year
    pieces.append(frame)

names = pd.concat(pieces, ignore_index=True)
names
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mary</td>
      <td>F</td>
      <td>7065</td>
      <td>1880</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anna</td>
      <td>F</td>
      <td>2604</td>
      <td>1880</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Emma</td>
      <td>F</td>
      <td>2003</td>
      <td>1880</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Elizabeth</td>
      <td>F</td>
      <td>1939</td>
      <td>1880</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Minnie</td>
      <td>F</td>
      <td>1746</td>
      <td>1880</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1690779</th>
      <td>Zymaire</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1690780</th>
      <td>Zyonne</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1690781</th>
      <td>Zyquarius</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1690782</th>
      <td>Zyran</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1690783</th>
      <td>Zzyzx</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
    </tr>
  </tbody>
</table>
<p>1690784 rows × 4 columns</p>
</div>




```python
total_births = names.pivot_table('births', index = 'year', columns = 'gender', aggfunc = sum)
total_births.plot(title = 'Total births(gender / year)',figsize=(10,10))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16a32d46310>




    
![](/img/output_4_1.svg)
    



```python
def add_prop(group) :
    group['prop'] = group.births / group.births.sum()
    return group
```


```python
names = names.groupby(['year', 'gender']).apply(add_prop)
names
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mary</td>
      <td>F</td>
      <td>7065</td>
      <td>1880</td>
      <td>0.077643</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anna</td>
      <td>F</td>
      <td>2604</td>
      <td>1880</td>
      <td>0.028618</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Emma</td>
      <td>F</td>
      <td>2003</td>
      <td>1880</td>
      <td>0.022013</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Elizabeth</td>
      <td>F</td>
      <td>1939</td>
      <td>1880</td>
      <td>0.021309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Minnie</td>
      <td>F</td>
      <td>1746</td>
      <td>1880</td>
      <td>0.019188</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1690779</th>
      <td>Zymaire</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
      <td>0.000003</td>
    </tr>
    <tr>
      <th>1690780</th>
      <td>Zyonne</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
      <td>0.000003</td>
    </tr>
    <tr>
      <th>1690781</th>
      <td>Zyquarius</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
      <td>0.000003</td>
    </tr>
    <tr>
      <th>1690782</th>
      <td>Zyran</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
      <td>0.000003</td>
    </tr>
    <tr>
      <th>1690783</th>
      <td>Zzyzx</td>
      <td>M</td>
      <td>5</td>
      <td>2010</td>
      <td>0.000003</td>
    </tr>
  </tbody>
</table>
<p>1690784 rows × 5 columns</p>
</div>




```python
names.groupby(['year', 'gender']).prop.sum()
```




    year  gender
    1880  F         1.0
          M         1.0
    1881  F         1.0
          M         1.0
    1882  F         1.0
                   ... 
    2008  M         1.0
    2009  F         1.0
          M         1.0
    2010  F         1.0
          M         1.0
    Name: prop, Length: 262, dtype: float64



## 연도별 / 성별에 따른 선호하는 이름 1000개 추출


```python
def get_top1000(group) :
    return group.sort_values(by = 'births', ascending = False)[:1000]
```


```python
grouped = names.groupby(['year', 'gender'])
top1000 = grouped.apply(get_top1000)
top1000
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
      <th></th>
      <th></th>
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
    <tr>
      <th>year</th>
      <th>gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">1880</th>
      <th rowspan="5" valign="top">F</th>
      <th>0</th>
      <td>Mary</td>
      <td>F</td>
      <td>7065</td>
      <td>1880</td>
      <td>0.077643</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anna</td>
      <td>F</td>
      <td>2604</td>
      <td>1880</td>
      <td>0.028618</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Emma</td>
      <td>F</td>
      <td>2003</td>
      <td>1880</td>
      <td>0.022013</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Elizabeth</td>
      <td>F</td>
      <td>1939</td>
      <td>1880</td>
      <td>0.021309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Minnie</td>
      <td>F</td>
      <td>1746</td>
      <td>1880</td>
      <td>0.019188</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">2010</th>
      <th rowspan="5" valign="top">M</th>
      <th>1677639</th>
      <td>Camilo</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>1677640</th>
      <td>Destin</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>1677641</th>
      <td>Jaquan</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>1677642</th>
      <td>Jaydan</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>1677645</th>
      <td>Maxton</td>
      <td>M</td>
      <td>193</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
  </tbody>
</table>
<p>261877 rows × 5 columns</p>
</div>




```python
top1000.reset_index(inplace=True, drop=True)
top1000
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mary</td>
      <td>F</td>
      <td>7065</td>
      <td>1880</td>
      <td>0.077643</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anna</td>
      <td>F</td>
      <td>2604</td>
      <td>1880</td>
      <td>0.028618</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Emma</td>
      <td>F</td>
      <td>2003</td>
      <td>1880</td>
      <td>0.022013</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Elizabeth</td>
      <td>F</td>
      <td>1939</td>
      <td>1880</td>
      <td>0.021309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Minnie</td>
      <td>F</td>
      <td>1746</td>
      <td>1880</td>
      <td>0.019188</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>261872</th>
      <td>Camilo</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261873</th>
      <td>Destin</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261874</th>
      <td>Jaquan</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261875</th>
      <td>Jaydan</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261876</th>
      <td>Maxton</td>
      <td>M</td>
      <td>193</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
  </tbody>
</table>
<p>261877 rows × 5 columns</p>
</div>



## 상위 1000개의 이름데이터를 남자(boys)와 여자(grils)로 분리


```python
boys = top1000[top1000.gender == 'M']
girls = top1000[top1000.gender == 'F']
```

## 연도와 출생수를 피봇데이블로 변환


```python
total_births = top1000.pivot_table('births', index='year', columns='name', aggfunc=sum)

```


```python
total_births.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 131 entries, 1880 to 2010
    Columns: 6868 entries, Aaden to Zuri
    dtypes: float64(6868)
    memory usage: 6.9 MB
    


```python
subset = total_births[['Jacob', 'Sophia', 'Michael', 'Alice']]
subset.plot(subplots=True, figsize=(10,10), grid=False, title='Number of birth per year')
```




    array([<matplotlib.axes._subplots.AxesSubplot object at 0x0000016A356BC400>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x0000016A35752850>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x0000016A3FD20AC0>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x0000016A3FD4EC40>],
          dtype=object)




    
![img](/img/output_17_1.png)
    



```python
import matplotlib.pyplot as plt
plt.figure()
table = top1000.pivot_table('prop', index='year', columns='gender', aggfunc=sum)
table.head()
# plt.rcParams["figure.figsize"] = (10,10) # 크기가 고정되버림 기본값 안돌아감
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
      <th>gender</th>
      <th>F</th>
      <th>M</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1880</th>
      <td>1.000000</td>
      <td>0.997375</td>
    </tr>
    <tr>
      <th>1881</th>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>1882</th>
      <td>0.998702</td>
      <td>0.995646</td>
    </tr>
    <tr>
      <th>1883</th>
      <td>0.997596</td>
      <td>0.998566</td>
    </tr>
    <tr>
      <th>1884</th>
      <td>0.993156</td>
      <td>0.994539</td>
    </tr>
  </tbody>
</table>
</div>




    <Figure size 432x288 with 0 Axes>



```python
table.plot(title='Sum of table1000.prop by year and sex',
           yticks=np.linspace(0, 1.2, 13), xticks=range(1880, 2020, 10), figsize=(10,10))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16a3fe274c0>




    
![img](/img/output_19_1.png)
    



```python
df = boys[boys.year==2010]
df
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>260877</th>
      <td>Jacob</td>
      <td>M</td>
      <td>21875</td>
      <td>2010</td>
      <td>0.011523</td>
    </tr>
    <tr>
      <th>260878</th>
      <td>Ethan</td>
      <td>M</td>
      <td>17866</td>
      <td>2010</td>
      <td>0.009411</td>
    </tr>
    <tr>
      <th>260879</th>
      <td>Michael</td>
      <td>M</td>
      <td>17133</td>
      <td>2010</td>
      <td>0.009025</td>
    </tr>
    <tr>
      <th>260880</th>
      <td>Jayden</td>
      <td>M</td>
      <td>17030</td>
      <td>2010</td>
      <td>0.008971</td>
    </tr>
    <tr>
      <th>260881</th>
      <td>William</td>
      <td>M</td>
      <td>16870</td>
      <td>2010</td>
      <td>0.008887</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>261872</th>
      <td>Camilo</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261873</th>
      <td>Destin</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261874</th>
      <td>Jaquan</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261875</th>
      <td>Jaydan</td>
      <td>M</td>
      <td>194</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
    <tr>
      <th>261876</th>
      <td>Maxton</td>
      <td>M</td>
      <td>193</td>
      <td>2010</td>
      <td>0.000102</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 5 columns</p>
</div>




```python
prop_cumsum = df.sort_values(by='prop', ascending=False).prop.cumsum()
prop_cumsum[:10]
prop_cumsum.values.searchsorted(0.5)+1
```




    117




```python
df = boys[boys.year==1910]
y1910 = df.sort_values(by='prop', ascending=False).prop.cumsum()
y1910.values.searchsorted(0.5)+1
```




    31




```python
def get_quantile_count(group, q=0.5):
    group = group.sort_values(by='prop', ascending=False)
    return group.prop.cumsum().values.searchsorted(q) + 1

diversity = top1000.groupby(['year', 'gender']).apply(get_quantile_count)
diversity = diversity.unstack('gender')
fig = plt.figure()
diversity.plot(title="Number of popular names in top 50%", figsize=(10,10))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16a4024e970>




    <Figure size 432x288 with 0 Axes>



    
![img](/img/output_23_2.png)
    


## 마지막 글자의 변화


```python
get_last_letter = lambda x : x[-1]
last_letters = names.name.map(get_last_letter)
last_letters.name = 'last_letter'
 
table = names.pivot_table('births', index=last_letters, columns=['gender', 'year'], aggfunc=sum)
```


```python
subtable = table.reindex(columns=[1910,1960,2010], level='year')
subtable.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>gender</th>
      <th colspan="3" halign="left">F</th>
      <th colspan="3" halign="left">M</th>
    </tr>
    <tr>
      <th>year</th>
      <th>1910</th>
      <th>1960</th>
      <th>2010</th>
      <th>1910</th>
      <th>1960</th>
      <th>2010</th>
    </tr>
    <tr>
      <th>last_letter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>108376.0</td>
      <td>691247.0</td>
      <td>670605.0</td>
      <td>977.0</td>
      <td>5204.0</td>
      <td>28438.0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>NaN</td>
      <td>694.0</td>
      <td>450.0</td>
      <td>411.0</td>
      <td>3912.0</td>
      <td>38859.0</td>
    </tr>
    <tr>
      <th>c</th>
      <td>5.0</td>
      <td>49.0</td>
      <td>946.0</td>
      <td>482.0</td>
      <td>15476.0</td>
      <td>23125.0</td>
    </tr>
    <tr>
      <th>d</th>
      <td>6750.0</td>
      <td>3729.0</td>
      <td>2607.0</td>
      <td>22111.0</td>
      <td>262112.0</td>
      <td>44398.0</td>
    </tr>
    <tr>
      <th>e</th>
      <td>133569.0</td>
      <td>435013.0</td>
      <td>313833.0</td>
      <td>28655.0</td>
      <td>178823.0</td>
      <td>129012.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
subtable.sum()
letter_prop = subtable / subtable.sum()
letter_prop
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>gender</th>
      <th colspan="3" halign="left">F</th>
      <th colspan="3" halign="left">M</th>
    </tr>
    <tr>
      <th>year</th>
      <th>1910</th>
      <th>1960</th>
      <th>2010</th>
      <th>1910</th>
      <th>1960</th>
      <th>2010</th>
    </tr>
    <tr>
      <th>last_letter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.273390</td>
      <td>0.341853</td>
      <td>0.381240</td>
      <td>0.005031</td>
      <td>0.002440</td>
      <td>0.014980</td>
    </tr>
    <tr>
      <th>b</th>
      <td>NaN</td>
      <td>0.000343</td>
      <td>0.000256</td>
      <td>0.002116</td>
      <td>0.001834</td>
      <td>0.020470</td>
    </tr>
    <tr>
      <th>c</th>
      <td>0.000013</td>
      <td>0.000024</td>
      <td>0.000538</td>
      <td>0.002482</td>
      <td>0.007257</td>
      <td>0.012181</td>
    </tr>
    <tr>
      <th>d</th>
      <td>0.017028</td>
      <td>0.001844</td>
      <td>0.001482</td>
      <td>0.113858</td>
      <td>0.122908</td>
      <td>0.023387</td>
    </tr>
    <tr>
      <th>e</th>
      <td>0.336941</td>
      <td>0.215133</td>
      <td>0.178415</td>
      <td>0.147556</td>
      <td>0.083853</td>
      <td>0.067959</td>
    </tr>
    <tr>
      <th>f</th>
      <td>NaN</td>
      <td>0.000010</td>
      <td>0.000055</td>
      <td>0.000783</td>
      <td>0.004325</td>
      <td>0.001188</td>
    </tr>
    <tr>
      <th>g</th>
      <td>0.000144</td>
      <td>0.000157</td>
      <td>0.000374</td>
      <td>0.002250</td>
      <td>0.009488</td>
      <td>0.001404</td>
    </tr>
    <tr>
      <th>h</th>
      <td>0.051529</td>
      <td>0.036224</td>
      <td>0.075852</td>
      <td>0.045562</td>
      <td>0.037907</td>
      <td>0.051670</td>
    </tr>
    <tr>
      <th>i</th>
      <td>0.001526</td>
      <td>0.039965</td>
      <td>0.031734</td>
      <td>0.000844</td>
      <td>0.000603</td>
      <td>0.022628</td>
    </tr>
    <tr>
      <th>j</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000090</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000769</td>
    </tr>
    <tr>
      <th>k</th>
      <td>0.000121</td>
      <td>0.000156</td>
      <td>0.000356</td>
      <td>0.036581</td>
      <td>0.049384</td>
      <td>0.018541</td>
    </tr>
    <tr>
      <th>l</th>
      <td>0.043189</td>
      <td>0.033867</td>
      <td>0.026356</td>
      <td>0.065016</td>
      <td>0.104904</td>
      <td>0.070367</td>
    </tr>
    <tr>
      <th>m</th>
      <td>0.001201</td>
      <td>0.008613</td>
      <td>0.002588</td>
      <td>0.058044</td>
      <td>0.033827</td>
      <td>0.024657</td>
    </tr>
    <tr>
      <th>n</th>
      <td>0.079240</td>
      <td>0.130687</td>
      <td>0.140210</td>
      <td>0.143415</td>
      <td>0.152522</td>
      <td>0.362771</td>
    </tr>
    <tr>
      <th>o</th>
      <td>0.001660</td>
      <td>0.002439</td>
      <td>0.001243</td>
      <td>0.017065</td>
      <td>0.012829</td>
      <td>0.042681</td>
    </tr>
    <tr>
      <th>p</th>
      <td>0.000018</td>
      <td>0.000023</td>
      <td>0.000020</td>
      <td>0.003172</td>
      <td>0.005675</td>
      <td>0.001269</td>
    </tr>
    <tr>
      <th>q</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000030</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000180</td>
    </tr>
    <tr>
      <th>r</th>
      <td>0.013390</td>
      <td>0.006764</td>
      <td>0.018025</td>
      <td>0.064481</td>
      <td>0.031034</td>
      <td>0.087477</td>
    </tr>
    <tr>
      <th>s</th>
      <td>0.039042</td>
      <td>0.012764</td>
      <td>0.013332</td>
      <td>0.130815</td>
      <td>0.102730</td>
      <td>0.065145</td>
    </tr>
    <tr>
      <th>t</th>
      <td>0.027438</td>
      <td>0.015201</td>
      <td>0.007830</td>
      <td>0.072879</td>
      <td>0.065655</td>
      <td>0.022861</td>
    </tr>
    <tr>
      <th>u</th>
      <td>0.000684</td>
      <td>0.000574</td>
      <td>0.000417</td>
      <td>0.000124</td>
      <td>0.000057</td>
      <td>0.001221</td>
    </tr>
    <tr>
      <th>v</th>
      <td>NaN</td>
      <td>0.000060</td>
      <td>0.000117</td>
      <td>0.000113</td>
      <td>0.000037</td>
      <td>0.001434</td>
    </tr>
    <tr>
      <th>w</th>
      <td>0.000020</td>
      <td>0.000031</td>
      <td>0.001182</td>
      <td>0.006329</td>
      <td>0.007711</td>
      <td>0.016148</td>
    </tr>
    <tr>
      <th>x</th>
      <td>0.000015</td>
      <td>0.000037</td>
      <td>0.000727</td>
      <td>0.003965</td>
      <td>0.001851</td>
      <td>0.008614</td>
    </tr>
    <tr>
      <th>y</th>
      <td>0.110972</td>
      <td>0.152569</td>
      <td>0.116828</td>
      <td>0.077349</td>
      <td>0.160987</td>
      <td>0.058168</td>
    </tr>
    <tr>
      <th>z</th>
      <td>0.002439</td>
      <td>0.000659</td>
      <td>0.000704</td>
      <td>0.000170</td>
      <td>0.000184</td>
      <td>0.001831</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(2, 1, figsize=(10,10))
letter_prop['M'].plot(kind='bar', rot=0, ax=axes[0], title='Male')
letter_prop['F'].plot(kind='bar', rot=0, ax=axes[1], title='Female',
                      legend=False,)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16a402fdd00>




    
![img](/img/output_28_1.png)
    



```python
letter_prop = table / table.sum()
dny_ts = letter_prop.loc[['d', 'n', 'y'],'M'].T
dny_ts.head()
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
      <th>last_letter</th>
      <th>d</th>
      <th>n</th>
      <th>y</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1880</th>
      <td>0.083055</td>
      <td>0.153213</td>
      <td>0.075760</td>
    </tr>
    <tr>
      <th>1881</th>
      <td>0.083247</td>
      <td>0.153214</td>
      <td>0.077451</td>
    </tr>
    <tr>
      <th>1882</th>
      <td>0.085340</td>
      <td>0.149560</td>
      <td>0.077537</td>
    </tr>
    <tr>
      <th>1883</th>
      <td>0.084066</td>
      <td>0.151646</td>
      <td>0.079144</td>
    </tr>
    <tr>
      <th>1884</th>
      <td>0.086120</td>
      <td>0.149915</td>
      <td>0.080405</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.close()
fig = plt.figure()
dny_ts.plot(figsize=(10,10))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16a3fd10fd0>




    <Figure size 432x288 with 0 Axes>



    
![img](/img/output_30_2.png)
    


## 남자이름 > 여자이름
## Lesley or Leslie
## 공통부분 : 'Lesl'


```python
all_names = pd.Series(top1000.name.unique())
lesley_like = all_names[all_names.str.lower().str.contains('lesl')]
lesley_like
```




    632     Leslie
    2294    Lesley
    4262    Leslee
    4728     Lesli
    6103     Lesly
    dtype: object




```python
filtered = top1000[top1000.name.isin(lesley_like)]
filtered.groupby('name').births.sum()
```




    name
    Leslee      1082
    Lesley     35022
    Lesli        929
    Leslie    370429
    Lesly      10067
    Name: births, dtype: int64




```python
table = filtered.pivot_table('births', index='year',
                             columns='gender', aggfunc='sum')
table = table.div(table.sum(1), axis=0)
table.tail()
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
      <th>gender</th>
      <th>F</th>
      <th>M</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2006</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure()
table.plot(style={'M': 'c-', 'F': 'm--'}, figsize=(10,10)) # c cyan m 마젠타
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16a4024e070>




    <Figure size 432x288 with 0 Axes>



    
![img](/img/output_35_2.png)
    



```python

```
