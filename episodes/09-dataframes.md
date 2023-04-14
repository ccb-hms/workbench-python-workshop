---
title: "Pandas DataFrames"
teaching: 20
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- "How can I perform statistical analysis of tabular data?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Select individual values from a Pandas dataframe."
- "Select entire rows or entire columns from a dataframe."
- "Select a subset of both rows and columns from a dataframe in a single operation."
- "Select a subset of a dataframe by a single Boolean criterion."

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- "Use `DataFrame.iloc[..., ...]` to select values by integer location."
- "Use `:` on its own to mean all columns or all rows."
- "Select multiple columns or rows using `DataFrame.loc` and a named slice."
- "Result of slicing can be used in further operations."
- "Use comparisons to select data based on value."
- "Select values or NaN using a Boolean mask."

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
As opposed to getting the top 10 genes across all samples, we want to instead get genes which have high expression for each timepoint. 

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

To select 

## Group By: split-apply-combine

Finally, for each group in the `wealth_score` table, we sum their (financial) contribution
across the years surveyed using chained methods:

```python
print(rnaseq_df.groupby(wealth_score).sum())
```

```output
          gdpPercap_1952  gdpPercap_1957  gdpPercap_1962  gdpPercap_1967  \
0.000000    36916.854200    46110.918793    56850.065437    71324.848786   
0.333333    16790.046878    20942.456800    25744.935321    33567.667670   
0.500000    11807.544405    14505.000150    18380.449470    21421.846200   
1.000000   104317.277560   127332.008735   149989.154201   178000.350040   

          gdpPercap_1972  gdpPercap_1977  gdpPercap_1982  gdpPercap_1987  \
0.000000    88569.346898   104459.358438   113553.768507   119649.599409   
0.333333    45277.839976    53860.456750    59679.634020    64436.912960   
0.500000    25377.727380    29056.145370    31914.712050    35517.678220   
1.000000   215162.343140   241143.412730   263388.781960   296825.131210   

          gdpPercap_1992  gdpPercap_1997  gdpPercap_2002  gdpPercap_2007  
0.000000    92380.047256   103772.937598   118590.929863   149577.357928  
0.333333    67918.093220    80876.051580   102086.795210   122803.729520  
0.500000    36310.666080    40723.538700    45564.308390    51403.028210  
1.000000   315238.235970   346930.926170   385109.939210   427850.333420
```


::: challenge
## Selection of Individual Values

Assume Pandas has been imported into your notebook
and the Gapminder GDP data for Europe has been loaded:

```python
import pandas as pd

df = pd.read_csv('data/gapminder_gdp_europe.csv', index_col='country')
```

Write an expression to find the Per Capita GDP of Serbia in 2007.

::: solution
The selection can be done by using the labels for both the row ("Serbia") and the column ("gdpPercap_2007"):
```python
print(df.loc['Serbia', 'gdpPercap_2007'])
```

The output is
```output
9786.534714
```
:::
:::

::: challenge
## Extent of Slicing

1.  Do the two statements below produce the same output?
2.  Based on this,
    what rule governs what is included (or not) in numerical slices and named slices in Pandas?

```python
print(df.iloc[0:2, 0:2])
print(df.loc['Albania':'Belgium', 'gdpPercap_1952':'gdpPercap_1962'])
```


::: solution
No, they do not produce the same output! The output of the first statement is:
```output
        gdpPercap_1952  gdpPercap_1957
country                                
Albania     1601.056136     1942.284244
Austria     6137.076492     8842.598030
```

The second statement gives:
```output
        gdpPercap_1952  gdpPercap_1957  gdpPercap_1962
country                                                
Albania     1601.056136     1942.284244     2312.888958
Austria     6137.076492     8842.598030    10750.721110
Belgium     8343.105127     9714.960623    10991.206760
```

Clearly, the second statement produces an additional column and an additional row compared to the first statement.  
What conclusion can we draw? We see that a numerical slice, 0:2, *omits* the final index (i.e. index 2)
in the range provided,
while a named slice, 'gdpPercap_1952':'gdpPercap_1962', *includes* the final element.
:::
:::

::: challenge
## Reconstructing Data

Explain what each line in the following short program does:
what is in `first`, `second`, etc.?

```python
first = pd.read_csv('data/gapminder_all.csv', index_col='country')
second = first[first['continent'] == 'Americas']
third = second.drop('Puerto Rico')
fourth = third.drop('continent', axis = 1)
fourth.to_csv('result.csv')
```


::: solution
Let's go through this piece of code line by line.
```python
first = pd.read_csv('data/gapminder_all.csv', index_col='country')
```

This line loads the dataset containing the GDP data from all countries into a dataframe called 
`first`. The `index_col='country'` parameter selects which column to use as the 
row labels in the dataframe.  
```python
second = first[first['continent'] == 'Americas']
```

This line makes a selection: only those rows of `first` for which the 'continent' column matches 
'Americas' are extracted. Notice how the Boolean expression inside the brackets, 
`first['continent'] == 'Americas'`, is used to select only those rows where the expression is true. 
Try printing this expression! Can you print also its individual True/False elements? 
(hint: first assign the expression to a variable)
```python
third = second.drop('Puerto Rico')
```

As the syntax suggests, this line drops the row from `second` where the label is 'Puerto Rico'. The 
resulting dataframe `third` has one row less than the original dataframe `second`.
```python
fourth = third.drop('continent', axis = 1)
```

Again we apply the drop function, but in this case we are dropping not a row but a whole column. 
To accomplish this, we need to specify also the `axis` parameter (we want to drop the second column 
which has index 1).
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
data = pd.read_csv('data/gapminder_gdp_europe.csv', index_col='country')
print(rnaseq_df.idxmin())
print(rnaseq_df.idxmax())
```


::: solution
For each column in `data`, `idxmin` will return the index value corresponding to each column's minimum;
`idxmax` will do accordingly the same for each column's maximum value.

You can use these functions whenever you want to get the row index of the minimum/maximum value and not the actual minimum/maximum value.
:::
:::

::: challenge
## Practice with Selection

Assume Pandas has been imported and the Gapminder GDP data for Europe has been loaded.
Write an expression to select each of the following:

1.  GDP per capita for all countries in 1982.
2.  GDP per capita for Denmark for all years.
3.  GDP per capita for all countries for years *after* 1985.
4.  GDP per capita for each country in 2007 as a multiple of 
    GDP per capita for that country in 1952.

::: solution
1:
```python
rnaseq_df['gdpPercap_1982']
```


2:
```python
rnaseq_df.loc['Denmark',:]
```


3:
```python
rnaseq_df.loc[:,'gdpPercap_1985':]
```

Pandas is smart enough to recognize the number at the end of the column label and does not give you an error, although no column named `gdpPercap_1985` actually exists. This is useful if new columns are added to the CSV file later.

4:
```python
rnaseq_df['gdpPercap_2007']/rnaseq_df['gdpPercap_1952']
```

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

::: challenge
## Exploring available methods using the `dir()` function

Python includes a `dir()` function that can be used to display all of the available methods (functions) that are built into a data object.  In Episode 4, we used some methods with a string. But we can see many more are available by using `dir()`:

```python
my_string = 'Hello world!'   # creation of a string object 
dir(my_string)
```


This command returns:

```output
['__add__',
...
'__subclasshook__',
'capitalize',
'casefold',
'center',
...
'upper',
'zfill']
```


You can use `help()` or <kbd>Shift</kbd>+<kbd>Tab</kbd> to get more information about what these methods do.

Assume Pandas has been imported and the Gapminder GDP data for Europe has been loaded as `data`.  Then, use `dir()` 
to find the function that prints out the median per-capita GDP across all European countries for each year that information is available.

::: solution
Among many choices, `dir()` lists the `median()` function as a possibility.  Thus,
```python
rnaseq_df.median()
```

:::
:::


[pandas-dataframe]: https://pandas.pyrnaseq_df.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pyrnaseq_df.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/
