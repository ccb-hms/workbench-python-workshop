---
title: "Reading tabular data"
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- "How can I read tabular data?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Import the Pandas library."
- "Use Pandas to load a simple CSV data set."
- "Get some basic information about a Pandas DataFrame."

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- "Use the Pandas library to get basic statistics out of tabular data."
- "Use `index_col` to specify that a column's values should be used as row headings."
- "Use `DataFrame.info` to find out more about a dataframe."
- "The `DataFrame.columns` variable stores information about the dataframe's columns."
- "Use `DataFrame.T` to transpose a dataframe."
- "Use `DataFrame.describe` to get summary statistics about data."

::::::::::::::::::::::::::::::::::::::::::::::::

## Use the Pandas library to do statistics on tabular data.

*   [Pandas](https://pandas.pydata.org/) is a widely-used Python library for statistics, particularly on tabular data.
*   Borrows many features from R's dataframes.
    *   A 2-dimensional table whose columns have names
        and potentially have different data types.
*   Load it with `import pandas as pd`. The alias pd is commonly used for Pandas.
*   Read a Comma Separated Values (CSV) data file with `pd.read_csv`.
    *   Argument is the name of the file to be read.
    *   Assign result to a variable to store the data that was read.

## Data description

We are going to use part of the data published by [Blackmore *et al.*
(2017)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5544260/), *The
effect of upper-respiratory infection on transcriptomic changes in the
CNS*. The goal of the study was to determine the effect of an
upper-respiratory infection on changes in RNA transcription occurring
in the cerebellum and spinal cord post infection. Gender matched eight
week old C57BL/6 mice were inoculated with saline or with Influenza A by
intranasal route and transcriptomic changes in the cerebellum and
spinal cord tissues were evaluated by RNA-seq at days 0
(non-infected), 4 and 8.

The dataset is stored as a comma-separated values (CSV) file.  Each row
holds information for a single RNA expression measurement, and the first eleven
columns represent:

| Column     | Description                                                                                  |
| ---------- | -------------------------------------------------------------------------------------------- |
| gene       | The name of the gene that was measured                                                       |
| sample     | The name of the sample the gene expression was measured in                                   |
| expression | The value of the gene expression                                                             |
| organism   | The organism/species - here all data stem from mice                                          |
| age        | The age of the mouse (all mice were 8 weeks here)                                            |
| sex        | The sex of the mouse                                                                         |
| infection  | The infection state of the mouse, i.e. infected with Influenza A or not infected.            |
| strain     | The Influenza A strain; C57BL/6 in all cases.                                                |
| time       | The duration of the infection (in days).                                                     |
| tissue     | The tissue that was used for the gene expression experiment, i.e. cerebellum or spinal cord. |
| mouse      | The mouse unique identifier.                                                                 |

For this lesson, we will only be using a subset of the total dataset. 

```python
import pandas as pd

url = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/rnaseq_reduced.csv"
rnaseq_df = pd.read_csv(url)
print(rnaseq_df)
```

```output
         gene      sample  expression  time
0         Asl  GSM2545336        1170     8
1        Apod  GSM2545336       36194     8
2     Cyp2d22  GSM2545336        4060     8
3        Klk6  GSM2545336         287     8
4       Fcrls  GSM2545336          85     8
...       ...         ...         ...   ...
7365    Mgst3  GSM2545340        1563     4
7366   Lrrc52  GSM2545340           2     4
7367     Rxrg  GSM2545340          26     4
7368    Lmx1a  GSM2545340          81     4
7369     Pbx1  GSM2545340        3805     4

[7370 rows x 4 columns]

```
*   The columns in a dataframe are the observed variables, and the rows are the observations.
*   Pandas uses backslash `\` to show wrapped lines when output is too wide to fit the screen.

## Use `index_col` to specify that a column's values should be used as row headings.

*   Row headings are numbers (0 and 1 in this case).
*   Really want to index by country.
*   Pass the name of the column to `read_csv` as its `index_col` parameter to do this.

```python
rnaseq_df = pd.read_csv(url, index_col='gene')
print(rnaseq_df)
```

```output
             sample  expression  time
gene                                 
Asl      GSM2545336        1170     8
Apod     GSM2545336       36194     8
Cyp2d22  GSM2545336        4060     8
Klk6     GSM2545336         287     8
Fcrls    GSM2545336          85     8
...             ...         ...   ...
Mgst3    GSM2545340        1563     4
Lrrc52   GSM2545340           2     4
Rxrg     GSM2545340          26     4
Lmx1a    GSM2545340          81     4
Pbx1     GSM2545340        3805     4

[7370 rows x 3 columns]
```


## Use the `DataFrame.info()` method to find out more about a dataframe.

```python
rnaseq_df.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
Index: 7370 entries, Asl to Pbx1
Data columns (total 3 columns):
 #   Column      Non-Null Count  Dtype 
---  ------      --------------  ----- 
 0   sample      7370 non-null   object
 1   expression  7370 non-null   int64 
 2   time        7370 non-null   int64 
dtypes: int64(2), object(1)
memory usage: 230.3+ KB
```

*   This is a `DataFrame`
*   It has 7370 rows, with the first rowname being Asl and the last bveing Pbx1
*   It has 3, two of which are 64-bit integer values.
    *   We will talk later about null values, which are used to represent missing observations.
*   Uses 230.3 KB of memory.

## The `DataFrame.columns` variable stores information about the dataframe's columns.

*   Note that this is data, *not* a method.  (It doesn't have parentheses.)
    *   Like `math.pi`.
    *   So do not use `()` to try to call it.
*   Called a *member variable*, or just *member*.

```python
print(rnaseq_df.columns)
```

```output
Index(['sample', 'expression', 'time'], dtype='object')
```


## Use `DataFrame.T` to transpose a dataframe.

*   Sometimes want to treat columns as rows and vice versa.
*   Transpose (written `.T`) doesn't copy the data, just changes the program's view of it.
*   Like `columns`, it is a member variable.

```python
print(rnaseq_df.T)
```

```output
gene               Asl        Apod     Cyp2d22        Klk6       Fcrls  \
sample      GSM2545336  GSM2545336  GSM2545336  GSM2545336  GSM2545336   
expression        1170       36194        4060         287          85   
time                 8           8           8           8           8   

gene            Slc2a4        Exd2        Gjc2        Plp1        Gnb4  ...  \
sample      GSM2545336  GSM2545336  GSM2545336  GSM2545336  GSM2545336  ...   
expression         782        1619         288       43217        1071  ...   
time                 8           8           8           8           8  ...   

gene            Dusp27        Mael     Gm16418     Gm16701     Aldh9a1  \
sample      GSM2545340  GSM2545340  GSM2545340  GSM2545340  GSM2545340   
expression          20           4          15         149        1462   
time                 4           4           4           4           4   

gene             Mgst3      Lrrc52        Rxrg       Lmx1a        Pbx1  
sample      GSM2545340  GSM2545340  GSM2545340  GSM2545340  GSM2545340  
expression        1563           2          26          81        3805  
time                 4           4           4           4           4  

[3 rows x 7370 columns]
```

## Use `DataFrame.describe()` to get summary statistics about data.

`DataFrame.describe()` gets the summary statistics of only the columns that have numerical data. 
All other columns are ignored, unless you use the argument `include='all'`.
```python
print(rnaseq_df.describe())
```

```output
          expression         time
count    7370.000000  7370.000000
mean     1774.729308     3.200000
std      4402.155716     2.993529
min         0.000000     0.000000
25%        65.000000     0.000000
50%       515.500000     4.000000
75%      1821.750000     4.000000
max    101241.000000     8.000000
```

::: challenge
## Inspecting Data

Use `help(rnaseq_df.head)` and `help(rnaseq_df.tail)`
to find out what `DataFrame.head` and `DataFrame.tail` do.

1.  What method call will display the first three rows of this data?
2.  Using `head` and/or `tail`, how would you display the last three columns of this data?
    (Hint: you may need to change your view of the data.)

::: solution
1. We can check out the first five rows of `rnaseq_df` by executing `rnaseq_df.head()`
   (allowing us to view the head of the DataFrame). We can specify the number of rows we wish
   to see by specifying the parameter `n` in our call
   to `rnaseq_df.head()`. To view the first three rows, execute:

   ```python
   rnaseq_df.head(n=3)
   ```

   ```output
 	        sample 	expression 	time
    gene 			
    Asl 	GSM2545336 	1170 	8
    Apod 	GSM2545336 	36194 	8
    Cyp2d22 GSM2545336 	4060 	8
   ```

2. To check out the last three rows of `rnaseq_df`, we would use the command,
   `rnaseq_df.tail(n=3)`, analogous to `head()` used above. However, here we want to look at
   the last three columns so we need to change our view and then use `tail()`. To do so, we
    create a new DataFrame in which rows and columns are switched:

   ```python
   rnaseq_df_flipped = rnaseq_df.T
   ```

   We can then view the last three columns of `rnaseq_df` by viewing the last three rows
   of `rnaseq_df_flipped`:
   ```python
   rnaseq_df_flipped.tail(n=3)
   ```

   ```output
 
    gene 	Asl 	Apod 	Cyp2d22 	Klk6 	Fcrls 	Slc2a4 	Exd2 	Gjc2 	Plp1 	Gnb4 	... 	Dusp27 	Mael 	Gm16418 	Gm16701 	Aldh9a1 	Mgst3 	Lrrc52 	Rxrg 	Lmx1a 	Pbx1
    sample 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	GSM2545336 	... 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340 	GSM2545340
    expression 	1170 	36194 	4060 	287 	85 	782 	1619 	288 	43217 	1071 	... 	20 	4 	15 	149 	1462 	1563 	2 	26 	81 	3805
    time 	8 	8 	8 	8 	8 	8 	8 	8 	8 	8 	... 	4 	4 	4 	4 	4 	4 	4 	4 	4 	4
   ```

   
   This shows the data that we want, but we may prefer to display three columns instead of three rows,
   so we can flip it back:
   ```python
   rnaseq_df_flipped.tail(n=3).T    
   ```

   ```output
       sample 	expression 	time
   gene 			
   Asl 	GSM2545336 	1170 	8
   Apod 	GSM2545336 	36194 	8
   Cyp2d22 	GSM2545336 	4060 	8
   Klk6 	GSM2545336 	287 	8
   Fcrls 	GSM2545336 	85 	8
   ... 	... 	... 	...
   Mgst3 	GSM2545340 	1563 	4
   Lrrc52 	GSM2545340 	2 	4
   Rxrg 	GSM2545340 	26 	4
   Lmx1a 	GSM2545340 	81 	4
   Pbx1 	GSM2545340 	3805 	4
   ```

   
   __Note:__ we could have done the above in a single line of code by 'chaining' the commands:
   ```python
   rnaseq_df.T.tail(n=3).T
   ```

:::
:::

::: challenge
## Writing Data

As well as the `read_csv` function for reading data from a file,
Pandas provides a `to_csv` function to write dataframes to files.
Applying what you've learned about reading from files,
write one of your dataframes to a file called `processed.csv`.
You can use `help` to get information on how to use `to_csv`.

::: solution
In order to write the DataFrame `rnaseq_df` to a file called `processed.csv`, execute the following command:
```python
rnaseq_df.to_csv('processed.csv')
```

For help on `to_csv`, you could execute, for example:
```python
help(rnaseq_df.to_csv)
```

Note that `help(to_csv)` throws an error! This is a subtlety and is due to the fact that `to_csv` is NOT a function in 
and of itself and the actual call is `rnaseq_df.to_csv`. 
:::
:::
