---
title: "Pandas DataFrames"
teaching: 20
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I perform statistical analysis of tabular data?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Select individual values from a Pandas dataframe.
- Select entire rows or entire columns from a dataframe.
- Select a subset of both rows and columns from a dataframe in a single operation.
- Select a subset of a dataframe by a single Boolean criterion.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use `DataFrame.iloc[..., ...]` to select values by integer location.
- Use `:` on its own to mean all columns or all rows.
- Select multiple columns or rows using `DataFrame.loc` and a named slice.
- Result of slicing can be used in further operations.
- Use comparisons to select data based on value.
- Select values or NaN using a Boolean mask.

::::::::::::::::::::::::::::::::::::::::::::::::

## Note about Pandas DataFrames/Series

A [DataFrame][pandas-dataframe] is a collection of [Series][pandas-series];
The DataFrame is the way Pandas represents a table, and Series is the data-structure
Pandas use to represent a column.

Pandas is built on top of the [Numpy][numpy] library, which in practice means that
most of the methods defined for Numpy Arrays apply to Pandas Series/DataFrames.

What makes Pandas so attractive is the powerful interface to access individual records
of the table, proper handling of missing values, and relational-databases operations
between DataFrames.

## Data description

We will be using the same data as the previous lesson from [Blackmore *et al.*
(2017)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5544260/), *The
effect of upper-respiratory infection on transcriptomic changes in the
CNS*. 

However, this data file consists only of the expression data in a gene x sample matrix. 

## Selecting values

To access a value at the position `[i,j]` of a DataFrame, we have two options, depending on
what is the meaning of `i` in use.
Remember that a DataFrame provides an *index* as a way to identify the rows of the table;
a row, then, has a *position* inside the table as well as a *label*, which
uniquely identifies its *entry* in the DataFrame.

## Use `DataFrame.iloc[..., ...]` to select values by their (entry) position

*   Can specify location by numerical index analogously to 2D version of character selection in strings.

```python
import pandas as pd

url = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/expression_matrix.csv"
rnaseq_df = pd.read_csv(url, index_col=0)
print(rnaseq_df.iloc[0, 0])
```

```output
1230
```

Note how here we used the index of the gene column as opposed to its name. 
It is often safer to use names instead of indices so that your code is robust to changes in the data layout, but sometimes indices are necessary.

## Use `DataFrame.loc[..., ...]` to select values by their (entry) label.

*   Can specify location by row and/or column name.

```python
print(rnaseq_df.loc["Cyp2d22", "GSM2545351"])
```

```output
3678
```

## Use `:` on its own to mean all columns or all rows.

*   Just like Python's usual slicing notation.

```python
print(rnaseq_df.loc["Cyp2d22", :])
```

```output
GSM2545336    4060
GSM2545337    1616
GSM2545338    1603
GSM2545339    1901
GSM2545340    2171
GSM2545341    3349
GSM2545342    3122
GSM2545343    2008
GSM2545344    2254
GSM2545345    2277
GSM2545346    2985
GSM2545347    3452
GSM2545348    1883
GSM2545349    2014
GSM2545350    2417
GSM2545351    3678
GSM2545352    2920
GSM2545353    2216
GSM2545354    1821
GSM2545362    2842
GSM2545363    2011
GSM2545380    4019
Name: Cyp2d22, dtype: int64
```

*   We would get the same result printing `rnaseq_df.loc["Albania"]` (without a second index).

```python
print(rnaseq_df.loc[:, "GSM2545351"])
```

```output
gene
AI504432    1136
AW046200      67
AW551984     584
Aamp        4813
Abca12         4
            ... 
Zkscan3     1661
Zranb1      8223
Zranb3       208
Zscan22      433
Zw10        1436
Name: GSM2545351, Length: 1474, dtype: int64
```


*   We would get the same result printing `rnaseq_df["GSM2545351"]`
*   We would also get the same result printing `rnaseq_df.GSM2545351` (not recommended, because easily confused with `.` notation for methods)

## Select multiple columns or rows using `DataFrame.loc` and a named slice.

```python
print(rnaseq_df.loc['Nadk':'Nid1', 'GSM2545337':'GSM2545341'])
```

```output
         GSM2545337  GSM2545338  GSM2545339  GSM2545340  GSM2545341
gene                                                               
Nadk           2285        2237        2286        2398        2201
Nap1l5        18287       17176       21950       18655       19160
Nbeal1         2230        2030        2053        1970        1910
Nbl1           2033        1859        1772        2439        3720
Ncf2             76          60          83          73          61
Nck2            683         706         690         644         648
Ncmap            37          40          46          51         138
Ncoa2          5053        4374        4406        4814        5311
Ndufa10        4218        3921        4268        3980        3631
Ndufs1         7042        6672        7413        7090        6943
Neto1           214         183         383         217         164
Nfkbia          740         897         724         873        1000
Nfya           1003         962         944         919         587
Ngb              56          46         135          63          50
Ngef            498         407         587         410         193
Ngp              50          18         131          47          27
Nid1           1521        1395         862         795         673

```


In the above code, we discover that **slicing using `loc` is inclusive at both
ends**, which differs from **slicing using `iloc`**, where slicing indicates
everything up to but not including the final index. 


## Result of slicing can be used in further operations.

*   Usually don't just print a slice.
*   All the statistical operators that work on entire dataframes
    work the same way on slices.
*   E.g., calculate max of a slice.

```python
print(rnaseq_df.loc['Nadk':'Nid1', 'GSM2545337':'GSM2545341'].max())
```

```output
GSM2545337    18287
GSM2545338    17176
GSM2545339    21950
GSM2545340    18655
GSM2545341    19160
dtype: int64
```


```python
print(rnaseq_df.loc['Nadk':'Nid1', 'GSM2545337':'GSM2545341'].min())
```

```output
GSM2545337    37
GSM2545338    18
GSM2545339    46
GSM2545340    47
GSM2545341    27
dtype: int64
```


## Use comparisons to select data based on value.

*   Comparison is applied element by element.
*   Returns a similarly-shaped dataframe of `True` and `False`.

```python
# Use a subset of data to keep output readable.
subset = rnaseq_df.iloc[:5, :5]
print('Subset of data:\n', subset)

# Which values were greater than 1000 ?
print('\nWhere are values large?\n', subset > 1000)
```

```output
Subset of data:
           GSM2545336  GSM2545337  GSM2545338  GSM2545339  GSM2545340
gene                                                                
AI504432        1230        1085         969        1284         966
AW046200          83         144         129         121         141
AW551984         670         265         202         682         246
Aamp            5621        4049        3797        4375        4095
Abca12             5           8           1           5           3

Where are values large?
           GSM2545336  GSM2545337  GSM2545338  GSM2545339  GSM2545340
gene                                                                
AI504432        True        True       False        True       False
AW046200       False       False       False       False       False
AW551984       False       False       False       False       False
Aamp            True        True        True        True        True
Abca12         False       False       False       False       False
```


## Select values or NaN using a Boolean mask.

*   A frame full of Booleans is sometimes called a *mask* because of how it can be used.

```python
mask = subset > 1000
print(subset[mask])
```

```output
          GSM2545336  GSM2545337  GSM2545338  GSM2545339  GSM2545340
gene                                                                
AI504432      1230.0      1085.0         NaN      1284.0         NaN
AW046200         NaN         NaN         NaN         NaN         NaN
AW551984         NaN         NaN         NaN         NaN         NaN
Aamp          5621.0      4049.0      3797.0      4375.0      4095.0
Abca12           NaN         NaN         NaN         NaN         NaN
```


*   Get the value where the mask is true, and NaN (Not a Number) where it is false.
*   **NaNs are ignored by operations like max, min, average, etc.** This is different than in other programming languages such as R but makes using masks in this way very useful. 
*   The behavior towards NaN values can be changed with the `skipna` argument for most pandas methods and functions.

```python
print(subset[subset > 1000].describe())
```

```output
        GSM2545336   GSM2545337  GSM2545338   GSM2545339  GSM2545340
count     2.000000     2.000000         1.0     2.000000         1.0
mean   3425.500000  2567.000000      3797.0  2829.500000      4095.0
std    3104.905876  2095.864499         NaN  2185.667061         NaN
min    1230.000000  1085.000000      3797.0  1284.000000      4095.0
25%    2327.750000  1826.000000      3797.0  2056.750000      4095.0
50%    3425.500000  2567.000000      3797.0  2829.500000      4095.0
75%    4523.250000  3308.000000      3797.0  3602.250000      4095.0
max    5621.000000  4049.000000      3797.0  4375.000000      4095.0
```

::: discussion

Why are there still `NaN`s in this output, when in Pandas methods like `min` and `max` igonre `Nan` by default?

::: solution
We still see `NaN` because GSM2545337 and GSM2545340 only have a single value over 1000 in this subset. The standard deviation (`std`) is undefined for a single value. 
:::
:::

## Using multiple dataframes

Pandas vectorizing methods and grouping operations are features that provide users 
much flexibility to analyse their data.

For instance, let's say we want to take a look at genes which are highly expressed over time.

To start, let's take a look at the top genes by mean expression.
First we calculate mean expression per gene:

```python
mean_exp = rnaseq_df.mean()
print(mean_exp.head())
```

```output
GSM2545336    2062.191995
GSM2545337    1765.508820
GSM2545338    1667.990502
GSM2545339    1696.120760
GSM2545340    1681.834464
dtype: float64
```

Notice that that this is printing differently than when we printed `Dataframe`s.
Methods which return single values such as `mean` by default return a `Series` instead of a `Dataframe`. 

```python
print(type(mean_exp))
```

```output
<class 'pandas.core.series.Series'>
```

We can think of a series as a single-column dataframe. 
Series, since they only contain a single column, are indexed like lists: `series[start:end]`. 

Now, we can use the `sort_values` method and look at the top 10 genes. 
Note that `sort_values` when used on a `Dataframe` with multiple columns needs a `by` argument to specify what column it should be sorted by. 


```python
mean_exp = mean_exp.sort_values()
mean_exp[:10]
```

```output
GSM2545342    1594.116689
GSM2545341    1637.532564
GSM2545338    1667.990502
GSM2545340    1681.834464
GSM2545346    1692.774084
GSM2545339    1696.120760
GSM2545345    1700.161465
GSM2545344    1712.440299
GSM2545337    1765.508820
GSM2545347    1805.396201
dtype: float64
```

Now let's say we want to do something a bit more complicated. 
As opposed to getting the top 10 genes across all samples, we want to instead get genes which had the highest absolute expression change from 0 to 4 days. 

To do that, we need to load in the sample metadata as a separate dataframe:

```python
url = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/metadata.csv"
metadata = pd.read_csv(url, index_col=0)
print(metadata)
```

```output
                organism  age     sex    infection   strain  time      tissue  \
sample                                                                          
GSM2545336  Mus musculus    8  Female   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545337  Mus musculus    8  Female  NonInfected  C57BL/6     0  Cerebellum   
GSM2545338  Mus musculus    8  Female  NonInfected  C57BL/6     0  Cerebellum   
GSM2545339  Mus musculus    8  Female   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545340  Mus musculus    8    Male   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545341  Mus musculus    8    Male   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545342  Mus musculus    8  Female   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545343  Mus musculus    8    Male  NonInfected  C57BL/6     0  Cerebellum   
GSM2545344  Mus musculus    8  Female   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545345  Mus musculus    8    Male   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545346  Mus musculus    8    Male   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545347  Mus musculus    8    Male   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545348  Mus musculus    8  Female  NonInfected  C57BL/6     0  Cerebellum   
GSM2545349  Mus musculus    8    Male  NonInfected  C57BL/6     0  Cerebellum   
GSM2545350  Mus musculus    8    Male   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545351  Mus musculus    8  Female   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545352  Mus musculus    8  Female   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545353  Mus musculus    8  Female  NonInfected  C57BL/6     0  Cerebellum   
GSM2545354  Mus musculus    8    Male  NonInfected  C57BL/6     0  Cerebellum   
GSM2545362  Mus musculus    8  Female   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545363  Mus musculus    8    Male   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545380  Mus musculus    8  Female   InfluenzaA  C57BL/6     8  Cerebellum   

            mouse  
sample             
GSM2545336     14  
GSM2545337      9  
GSM2545338     10  
GSM2545339     15  
GSM2545340     18  
GSM2545341      6  
GSM2545342      5  
GSM2545343     11  
GSM2545344     22  
GSM2545345     13  
GSM2545346     23  
GSM2545347     24  
GSM2545348      8  
GSM2545349      7  
GSM2545350      1  
GSM2545351     16  
GSM2545352     21  
GSM2545353      4  
GSM2545354      2  
GSM2545362     20  
GSM2545363     12  
GSM2545380     19  

```
Time is a property of the samples, not of the genes. 
While we could add another row to our dataframe indicating time, it makes more sense to flip our dataframe and add it as a column.
We will learn about other ways to combine dataframes in a future lesson, but for now since `metadata` and `rnadeq_df` have the same index values we can use the following syntax:

```python
filled_df = rnaseq_df.T
flipped_df['time'] = metadata['time']
print(flipped_df.head())
```

```output
gene        AI504432  AW046200  AW551984  Aamp  Abca12  Abcc8  Abhd14a  Abi2  \
GSM2545336      1230        83       670  5621       5   2210      490  5627   
GSM2545337      1085       144       265  4049       8   1966      495  4383   
GSM2545338       969       129       202  3797       1   2181      474  4107   
GSM2545339      1284       121       682  4375       5   2362      468  4062   
GSM2545340       966       141       246  4095       3   2475      489  4289   

gene        Abi3bp  Abl2  ...  Zfp92  Zfp941  Zfyve28  Zgrf1  Zkscan3  Zranb1  \
GSM2545336     807  2392  ...     91     910     3747    644     1732    8837   
GSM2545337    1144  2133  ...     46     654     2568    335     1840    5306   
GSM2545338    1028  1891  ...     19     560     2635    347     1800    5106   
GSM2545339     935  1645  ...     50     782     2623    405     1751    5306   
GSM2545340     879  1926  ...     48     696     3140    549     2056    5896   

gene        Zranb3  Zscan22  Zw10  time  
GSM2545336     207      483  1479     8  
GSM2545337     179      535  1394     0  
GSM2545338     199      533  1279     0  
GSM2545339     208      462  1376     4  
GSM2545340     184      439  1568     4 
```

## Group By: split-apply-combine

We can now **group** our data by timepoint using `groupby`. 
`groupby` will combine our data by one or more columns, and then any summarizing functions we call such as `mean` or `max` will be performed on each group. 

```python
grouped_time = flipped_df.groupby('time')
print(grouped_time.mean())
```

```output
gene     AI504432    AW046200    AW551984         Aamp    Abca12        Abcc8  \
time                                                                            
0     1033.857143  155.285714  238.000000  4602.571429  5.285714  2576.428571   
4     1104.500000  152.375000  302.250000  4870.000000  4.250000  2608.625000   
8     1014.000000   81.000000  342.285714  4762.571429  4.142857  2291.571429   

gene     Abhd14a         Abi2       Abi3bp         Abl2  ...      Zfp810  \
time                                                     ...               
0     591.428571  4880.571429  1174.571429  2170.142857  ...  537.000000   
4     546.750000  4902.875000  1060.625000  2077.875000  ...  629.125000   
8     432.428571  4945.285714   762.285714  2131.285714  ...  940.428571   

gene      Zfp92      Zfp941      Zfyve28       Zgrf1      Zkscan3   Zranb1  \
time                                                                         
0     36.857143  673.857143  3162.428571  398.571429  2105.571429  6014.00   
4     43.000000  770.875000  3284.250000  450.125000  1981.500000  6464.25   
8     49.000000  765.571429  3649.571429  635.571429  1664.428571  8187.00   

gene      Zranb3     Zscan22         Zw10  
time                                       
0     191.142857  607.428571  1546.285714  
4     196.750000  534.500000  1555.125000  
8     191.428571  416.428571  1476.428571  

[3 rows x 1474 columns]
```

Another way to easily make a Dataframe after using `groupby` is to use Pandas' aggregation function called `agg`. 
Let's also flip the data back. 

```python
time_means = grouped_time.agg('mean').T
print(time_means.head())
```

```output
time                0         4            8
gene                                        
AI504432  1033.857143  1104.500  1014.000000
AW046200   155.285714   152.375    81.000000
AW551984   238.000000   302.250   342.285714
Aamp      4602.571429  4870.000  4762.571429
Abca12       5.285714     4.250     4.142857
```

Let's also make the column names someting more legible using `rename`.

```python
time_means = time_means.rename(columns={0: "day_0", 4: "day_4", 8: "day_8"})
print(time_means.head())
```

```output
time            day_0     day_4        day_8
gene                                        
AI504432  1033.857143  1104.500  1014.000000
AW046200   155.285714   152.375    81.000000
AW551984   238.000000   302.250   342.285714
Aamp      4602.571429  4870.000  4762.571429
Abca12       5.285714     4.250     4.142857
```

Now we can calculate the difference between 0 and 4 days:

```python
time_means['diff_4_0'] = time_means['day_4'] - time_means['day_0']
print(time_means.head())
```

```output
time            day_0     day_4        day_8    diff_4_0
gene                                                    
AI504432  1033.857143  1104.500  1014.000000   70.642857
AW046200   155.285714   152.375    81.000000   -2.910714
AW551984   238.000000   302.250   342.285714   64.250000
Aamp      4602.571429  4870.000  4762.571429  267.428571
Abca12       5.285714     4.250     4.142857   -1.035714
```

And get the top genes. 
This time we'll use the `nlargest` method instead of sorting:

```python
print(time_means.nlargest(10, 'diff_4_0'))
```

```output
time           day_0      day_4         day_8     diff_4_0
gene                                                      
Glul    48123.285714  55357.500  73947.857143  7234.214286
Sparc   35429.571429  38832.000  56105.714286  3402.428571
Atp1b1  57350.714286  60364.250  59229.000000  3013.535714
Apod    11575.428571  14506.500  31458.142857  2931.071429
Ttyh1   21453.571429  24252.500  30457.428571  2798.928571
Mt1      7601.428571  10112.375  14397.285714  2510.946429
Etnppl   4516.714286   6702.875   8208.142857  2186.160714
Kif5a   36079.571429  37911.750  33410.714286  1832.178571
Pink1   15454.571429  17252.375  23305.285714  1797.803571
Itih3    2467.714286   3976.500   5534.571429  1508.785714
```


::: challenge
## Creating new columns

We looked at the absolute expression change when finding top genes, but typically we actually want to look at the log (base 2) fold chage. 

The log fold change from `x` to `y` can be calculated as either:

$log(y) - log(x)$ 

or as 

$log(\frac{y}{x})$ 

Try calculating the log fold change from 0 to 4 and 0 to 8 days, and store the result as two new columns in `time_means`

We will need the `log2` function from the `numpy` package to do this:

```python
import numpy as np
np.log2(time_means['day_0'])
```

::: solution

```python
import numpy as np
time_means['logfc_0_4'] = np.log2(time_means['day_4']) - np.log2(time_means['day_0'])
time_means['logfc_0_8'] = np.log2(time_means['day_8']) - np.log2(time_means['day_0'])
print(time_means.head())
```

```output
time            day_0     day_4        day_8    diff_4_0  logfc_0_4  logfc_0_8
gene                                                                          
AI504432  1033.857143  1104.500  1014.000000   70.642857   0.095357  -0.027979
AW046200   155.285714   152.375    81.000000   -2.910714  -0.027299  -0.938931
AW551984   238.000000   302.250   342.285714   64.250000   0.344781   0.524240
Aamp      4602.571429  4870.000  4762.571429  267.428571   0.081482   0.049301
Abca12       5.285714     4.250     4.142857   -1.035714  -0.314636  -0.351472
```

:::
:::

::: challenge
## Extent of Slicing

1.  Do the two statements below produce the same output?
2.  Based on this,
    what rule governs what is included (or not) in numerical slices and named slices in Pandas?

Here's metadata as a reminder:

```
                organism  age     sex    infection   strain  time      tissue  \
sample                                                                          
GSM2545336  Mus musculus    8  Female   InfluenzaA  C57BL/6     8  Cerebellum   
GSM2545337  Mus musculus    8  Female  NonInfected  C57BL/6     0  Cerebellum   
GSM2545338  Mus musculus    8  Female  NonInfected  C57BL/6     0  Cerebellum   
GSM2545339  Mus musculus    8  Female   InfluenzaA  C57BL/6     4  Cerebellum   
GSM2545340  Mus musculus    8    Male   InfluenzaA  C57BL/6     4  Cerebellum   

            mouse  
sample             
GSM2545336     14  
GSM2545337      9  
GSM2545338     10  
GSM2545339     15  
GSM2545340     18  
```

```python
print(metadata.iloc[0:2, 0:2])
print(metadata.loc['GSM2545336':'GSM2545338', 'organism':'sex'])
```

::: solution
No, they do not produce the same output! The output of the first statement is:
```output
                organism  age
sample                       
GSM2545336  Mus musculus    8
GSM2545337  Mus musculus    8
```

The second statement gives:
```output
                organism  age     sex
sample                               
GSM2545336  Mus musculus    8  Female
GSM2545337  Mus musculus    8  Female
GSM2545338  Mus musculus    8  Female
```

Clearly, the second statement produces an additional column and an additional row compared to the first statement.  
What conclusion can we draw? We see that a numerical slice, 0:2, *omits* the final index (i.e. index 2)
in the range provided,
while a named slice, 'GSM2545336':'GSM2545338', *includes* the final element.
:::
:::

::: challenge
## Reconstructing Data

Explain what each line in the following short program does:
what is in `first`, `second`, etc.?

```python
first = metadata.copy(deep=True)
second = first[first['sex'] == 'Female']
third = second.drop('GSM2545338')
fourth = third.drop('sex', axis = 1)
fourth.to_csv('result.csv')
```


::: solution
Let's go through this piece of code line by line.
```python
first = metadata.copy(deep=True)
```

This line makes a copy of the metadata dataframe.
While we probably don't need to make sure `deep=True` here, as non of our columns are more complex or compound objects, it never hurts to be safe. 
 
```python
second = first[first['sex'] == 'Female']
```

This line makes a selection: only those rows of `first` for which the 'sex' column matches 
'Female' are extracted. Notice how the Boolean expression inside the brackets, 
`first['sex'] == 'Female'`, is used to select only those rows where the expression is true. 
Try printing this expression! Can you print also its individual True/False elements? 
(hint: first assign the expression to a variable)

```python
third = second.drop('GSM2545338')
```

As the syntax suggests, this line drops the row from `second` where the label is 'GSM2545338'. The 
resulting dataframe `third` has one row less than the original dataframe `second`.

```python
fourth = third.drop('sex', axis = 1)
```

Again we apply the drop function, but in this case we are dropping not a row but a whole column. 
To accomplish this, we need to specify also the `axis` parameter (axis is by default set to 0 for rows, and we change it to 1 for columns).

```python
fourth.to_csv('result.csv')
```

The final step is to write the data that we have been working on to a csv file. Pandas makes this easy 
with the `to_csv()` function. The only required argument to the function is the filename. Note that the 
file will be written in the directory from which you started the Jupyter or Python session.
:::
:::

::: challenge
## Selecting Indices

Explain in simple terms what `idxmin` and `idxmax` do in the short program below.
When would you use these methods?

```python
print("idxmin:")
print(rnaseq_df.idxmin())
print("idxmax:")
print(rnaseq_df.idxmax())
```


::: solution

```output
idxmin:
GSM2545336       Cfhr2
GSM2545337       Cfhr2
GSM2545338        Aox2
GSM2545339       Ascl5
GSM2545340       Ascl5
GSM2545341    BC055402
GSM2545342       Cryga
GSM2545343       Ascl5
GSM2545344        Aox2
GSM2545345    BC055402
GSM2545346        Cpa6
GSM2545347       Cfhr2
GSM2545348       Acmsd
GSM2545349     Fam124b
GSM2545350       Cryga
GSM2545351       Glrp1
GSM2545352       Cfhr2
GSM2545353       Ascl5
GSM2545354       Ascl5
GSM2545362       Cryga
GSM2545363    Adamts19
GSM2545380       Ascl5
dtype: object
idxmax:
GSM2545336    Glul
GSM2545337    Plp1
GSM2545338    Plp1
GSM2545339    Plp1
GSM2545340    Plp1
GSM2545341    Glul
GSM2545342    Glul
GSM2545343    Plp1
GSM2545344    Plp1
GSM2545345    Plp1
GSM2545346    Glul
GSM2545347    Glul
GSM2545348    Plp1
GSM2545349    Plp1
GSM2545350    Plp1
GSM2545351    Glul
GSM2545352    Plp1
GSM2545353    Plp1
GSM2545354    Plp1
GSM2545362    Plp1
GSM2545363    Plp1
GSM2545380    Glul
dtype: object
```

For each column in `data`, `idxmin` will return the index value corresponding to each column's minimum;
`idxmax` will do accordingly the same for each column's maximum value.

You can use these functions whenever you want to get the row index of the minimum/maximum value and not the actual minimum/maximum value.
:::
:::

::: challenge
## Many Ways of Access

There are at least two ways of accessing a value or slice of a DataFrame: by name or index.
However, there are many others. For example, a single column or row can be accessed either as a `DataFrame`
or a `Series` object.

Suggest different ways of doing the following operations on a DataFrame:
1. Access a single column
2. Access a single row
3. Access an individual DataFrame element
4. Access several columns
5. Access several rows
6. Access a subset of specific rows and columns
7. Access a subset of row and column ranges

::: solution
1\. Access a single column:
```python
# by name
rnaseq_df["col_name"]   # as a Series
rnaseq_df[["col_name"]] # as a DataFrame

# by name using .loc
rnaseq_df.T.loc["col_name"]  # as a Series
rnaseq_df.T.loc[["col_name"]].T  # as a DataFrame

# Dot notation (Series)
rnaseq_df.col_name

# by index (iloc)
rnaseq_df.iloc[:, col_index]   # as a Series
rnaseq_df.iloc[:, [col_index]] # as a DataFrame

# using a mask
rnaseq_df.T[rnaseq_df.T.index == "col_name"].T
```


2\. Access a single row:
```python
# by name using .loc
rnaseq_df.loc["row_name"] # as a Series
rnaseq_df.loc[["row_name"]] # as a DataFrame

# by name
rnaseq_df.T["row_name"] # as a Series
rnaseq_df.T[["row_name"]].T # as a DataFrame

# by index
rnaseq_df.iloc[row_index]   # as a Series
rnaseq_df.iloc[[row_index]]   # as a DataFrame

# using mask
rnaseq_df[rnaseq_df.index == "row_name"]
```


3\. Access an individual DataFrame element:
```python
# by column/row names
rnaseq_df["column_name"]["row_name"]         # as a Series

rnaseq_df[["col_name"]].loc["row_name"]  # as a Series
rnaseq_df[["col_name"]].loc[["row_name"]]  # as a DataFrame

rnaseq_df.loc["row_name"]["col_name"]  # as a value
rnaseq_df.loc[["row_name"]]["col_name"]  # as a Series
rnaseq_df.loc[["row_name"]][["col_name"]]  # as a DataFrame

rnaseq_df.loc["row_name", "col_name"]  # as a value
rnaseq_df.loc[["row_name"], "col_name"]  # as a Series. Preserves index. Column name is moved to `.name`.
rnaseq_df.loc["row_name", ["col_name"]]  # as a Series. Index is moved to `.name.` Sets index to column name.
rnaseq_df.loc[["row_name"], ["col_name"]]  # as a DataFrame (preserves original index and column name)

# by column/row names: Dot notation
rnaseq_df.col_name.row_name

# by column/row indices
rnaseq_df.iloc[row_index, col_index] # as a value
rnaseq_df.iloc[[row_index], col_index] # as a Series. Preserves index. Column name is moved to `.name`
rnaseq_df.iloc[row_index, [col_index]] # as a Series. Index is moved to `.name.` Sets index to column name.
rnaseq_df.iloc[[row_index], [col_index]] # as a DataFrame (preserves original index and column name)

# column name + row index
rnaseq_df["col_name"][row_index]
rnaseq_df.col_name[row_index]
rnaseq_df["col_name"].iloc[row_index]

# column index + row name
rnaseq_df.iloc[:, [col_index]].loc["row_name"]  # as a Series
rnaseq_df.iloc[:, [col_index]].loc[["row_name"]]  # as a DataFrame

# using masks
rnaseq_df[rnaseq_df.index == "row_name"].T[rnaseq_df.T.index == "col_name"].T
```

4\. Access several columns:
```python
# by name
rnaseq_df[["col1", "col2", "col3"]]
rnaseq_df.loc[:, ["col1", "col2", "col3"]]

# by index
rnaseq_df.iloc[:, [col1_index, col2_index, col3_index]]
```

5\. Access several rows
```python
# by name
rnaseq_df.loc[["row1", "row2", "row3"]]

# by index
rnaseq_df.iloc[[row1_index, row2_index, row3_index]]
```

6\. Access a subset of specific rows and columns
```python
# by names
rnaseq_df.loc[["row1", "row2", "row3"], ["col1", "col2", "col3"]]

# by indices
rnaseq_df.iloc[[row1_index, row2_index, row3_index], [col1_index, col2_index, col3_index]]

# column names + row indices
rnaseq_df[["col1", "col2", "col3"]].iloc[[row1_index, row2_index, row3_index]]

# column indices + row names
rnaseq_df.iloc[:, [col1_index, col2_index, col3_index]].loc[["row1", "row2", "row3"]]
```

7\. Access a subset of row and column ranges
```python
# by name
rnaseq_df.loc["row1":"row2", "col1":"col2"]

# by index
rnaseq_df.iloc[row1_index:row2_index, col1_index:col2_index]

# column names + row indices
rnaseq_df.loc[:, "col1_name":"col2_name"].iloc[row1_index:row2_index]

# column indices + row names
rnaseq_df.iloc[:, col1_index:col2_index].loc["row1":"row2"]
```
:::
:::

[pandas-dataframe]: https://pandas.pyrnaseq_df.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pyrnaseq_df.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/
