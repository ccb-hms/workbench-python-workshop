---
title: "Combining Data"
teaching: 10
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I combine dataframes?
- How do I handle missing or incomplete data mappings?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- 

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- 
::::::::::::::::::::::::::::::::::::::::::::::::

There are a variety of ways we might want to combine data when performing a data analysis. 
We can generally group these into **concatenating** (sometimes called *appending*) and **merging** (sometimes called *joining*). 

We will continue to use the rnaseq dataset:

```python
import pandas as pd

url = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/rnaseq.csv"
rnaseq_df = pd.read_csv(url, index_col=0)
```

## Concatenate dataframes to add additional *rows*. 

When we want to combine two dataframes by adding one as additional rows, we concatenate them together. 
This if often the case if our observations are spread out over multiple files. 
To simulate this, let's make two miniature versions of `rnaseq_df`` with the first and last 10 rows of the data:

```python
rnaseq_mini = rnaseq_df.loc[:,["sample", "expression"]].head(10)
rnaseq_mini_tail = rnaseq_df.loc[:,["sample", "expression"]].tail(10)
print(rnaseq_mini)
print(rnaseq_mini_tail)
```

```output
             sample  expression
gene                           
Asl      GSM2545336        1170
Apod     GSM2545336       36194
Cyp2d22  GSM2545336        4060
Klk6     GSM2545336         287
Fcrls    GSM2545336          85
Slc2a4   GSM2545336         782
Exd2     GSM2545336        1619
Gjc2     GSM2545336         288
Plp1     GSM2545336       43217
Gnb4     GSM2545336        1071
             sample  expression
gene                           
Dusp27   GSM2545380          15
Mael     GSM2545380           4
Gm16418  GSM2545380          16
Gm16701  GSM2545380         181
Aldh9a1  GSM2545380        1770
Mgst3    GSM2545380        2151
Lrrc52   GSM2545380           5
Rxrg     GSM2545380          49
Lmx1a    GSM2545380          72
Pbx1     GSM2545380        4795
```

We can then concatenate the dataframes using the pandas function `concat`. 

```python
combined_df = pd.concat([rnaseq_mini, rnaseq_mini_tail])
print(combined_df)
```

```output
             sample  expression
gene                           
Asl      GSM2545336        1170
Apod     GSM2545336       36194
Cyp2d22  GSM2545336        4060
Klk6     GSM2545336         287
Fcrls    GSM2545336          85
Slc2a4   GSM2545336         782
Exd2     GSM2545336        1619
Gjc2     GSM2545336         288
Plp1     GSM2545336       43217
Gnb4     GSM2545336        1071
Dusp27   GSM2545380          15
Mael     GSM2545380           4
Gm16418  GSM2545380          16
Gm16701  GSM2545380         181
Aldh9a1  GSM2545380        1770
Mgst3    GSM2545380        2151
Lrrc52   GSM2545380           5
Rxrg     GSM2545380          49
Lmx1a    GSM2545380          72
Pbx1     GSM2545380        4795
```

We now have 20 rows in our combined dataset, and the same number of columns. 
Note that `concat` is a *function* of the `pd` module, as opposed to a dataframe method. 
It takes in a list of dataframes, and outputs a combined dataframe. 

If one dataframe has columns which don't exist in the other, these values are filled in with `NaN`. 

```python
rnaseq_mini_time = rnaseq_df.loc[:,["sample", "expression","time"]].iloc[10:20,:]
print(rnaseq_mini_time)
```

```output
            sample  expression  time
gene                                
Tnc     GSM2545336         219     8
Trf     GSM2545336        9719     8
Tubb2b  GSM2545336        2245     8
Fads1   GSM2545336        6498     8
Lxn     GSM2545336        1744     8
Prr18   GSM2545336        1284     8
Cmtm5   GSM2545336        1381     8
Enpp1   GSM2545336         388     8
Clic4   GSM2545336        5795     8
Tm6sf2  GSM2545336          32     8
```

```python
mini_dfs = [rnaseq_mini, rnaseq_mini_time, rnaseq_mini_tail]
combined_df = pd.concat(mini_dfs)
print(combined_df)
```

```output
             sample  expression  time
gene                                 
Asl      GSM2545336        1170   NaN
Apod     GSM2545336       36194   NaN
Cyp2d22  GSM2545336        4060   NaN
Klk6     GSM2545336         287   NaN
Fcrls    GSM2545336          85   NaN
Slc2a4   GSM2545336         782   NaN
Exd2     GSM2545336        1619   NaN
Gjc2     GSM2545336         288   NaN
Plp1     GSM2545336       43217   NaN
Gnb4     GSM2545336        1071   NaN
Tnc      GSM2545336         219   8.0
Trf      GSM2545336        9719   8.0
Tubb2b   GSM2545336        2245   8.0
Fads1    GSM2545336        6498   8.0
Lxn      GSM2545336        1744   8.0
Prr18    GSM2545336        1284   8.0
Cmtm5    GSM2545336        1381   8.0
Enpp1    GSM2545336         388   8.0
Clic4    GSM2545336        5795   8.0
Tm6sf2   GSM2545336          32   8.0
Dusp27   GSM2545380          15   NaN
Mael     GSM2545380           4   NaN
Gm16418  GSM2545380          16   NaN
Gm16701  GSM2545380         181   NaN
Aldh9a1  GSM2545380        1770   NaN
Mgst3    GSM2545380        2151   NaN
Lrrc52   GSM2545380           5   NaN
Rxrg     GSM2545380          49   NaN
Lmx1a    GSM2545380          72   NaN
Pbx1     GSM2545380        4795   NaN
```

## Merge data frames to add additional *columns*. 

### Types of joins

### Missing data