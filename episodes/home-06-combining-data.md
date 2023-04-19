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

## Merge or join data frames to add additional *columns*. 

As opposed to concatenating data, we instead might want to add additional columns to a dataframe. 
We've already seen how to add a new column based on a calculation, but often we have some other data table we want to combine. 

If we know that the rows are in the same order, we can use the same syntax we use to assign new columns. 
However, this is often not the case.

```python
# There is an ongoing idelogical debate among developers whether variables like this should be named:
# url1, url2, and url3
# url0, url1, and url2 or
# url, url2, and url3
url1 = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/annot1.csv"
url2 = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/annot2.csv"
url3 = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/annot3.csv"

# Here .sample is being used to shuffle the dataframe rows into a random order
annot1 = pd.read_csv(url1, index_col=0).sample(frac=1)
annot2 = pd.read_csv(url2, index_col=0).sample(frac=1)
annot3 = pd.read_csv(url3, index_col=0).sample(frac=1)
print(annot1)
```

```output
                                          gene_description
gene                                                      
Fcrls    Fc receptor-like S, scavenger receptor [Source...
Plp1     proteolipid protein (myelin) 1 [Source:MGI Sym...
Apod     apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...
Klk6     kallikrein related-peptidase 6 [Source:MGI Sym...
Gnb4     guanine nucleotide binding protein (G protein)...
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...
Slc2a4   solute carrier family 2 (facilitated glucose t...
Asl      argininosuccinate lyase [Source:MGI Symbol;Acc...
Gjc2     gap junction protein, gamma 2 [Source:MGI Symb...
Exd2     exonuclease 3'-5' domain containing 2 [Source:...
```

```python
print(rnaseq_mini.join(annot1))
```

```output
             sample  expression  \
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

                                          gene_description  
gene                                                        
Asl      argininosuccinate lyase [Source:MGI Symbol;Acc...  
Apod     apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...  
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...  
Klk6     kallikrein related-peptidase 6 [Source:MGI Sym...  
Fcrls    Fc receptor-like S, scavenger receptor [Source...  
Slc2a4   solute carrier family 2 (facilitated glucose t...  
Exd2     exonuclease 3'-5' domain containing 2 [Source:...  
Gjc2     gap junction protein, gamma 2 [Source:MGI Symb...  
Plp1     proteolipid protein (myelin) 1 [Source:MGI Sym...  
Gnb4     guanine nucleotide binding protein (G protein)...  
```

We have combined the two dataframes to add the `gene_description` column. 
By default, `join` looks at the index column of the left and right dataframe, combining rows when it finds matches. 
The data used to determine which rows should be combined between the dataframes is referred to as what is being joined on. 
Here, we would say we are joining on the index columns. 
The row order and column order depends on which dataframe is on the left.

```python
print(annot1.join(rnaseq_mini))
```

```output
                                          gene_description      sample  \
gene                                                                     
Fcrls    Fc receptor-like S, scavenger receptor [Source...  GSM2545336   
Plp1     proteolipid protein (myelin) 1 [Source:MGI Sym...  GSM2545336   
Apod     apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...  GSM2545336   
Klk6     kallikrein related-peptidase 6 [Source:MGI Sym...  GSM2545336   
Gnb4     guanine nucleotide binding protein (G protein)...  GSM2545336   
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...  GSM2545336   
Slc2a4   solute carrier family 2 (facilitated glucose t...  GSM2545336   
Asl      argininosuccinate lyase [Source:MGI Symbol;Acc...  GSM2545336   
Gjc2     gap junction protein, gamma 2 [Source:MGI Symb...  GSM2545336   
Exd2     exonuclease 3'-5' domain containing 2 [Source:...  GSM2545336   

         expression  
gene                 
Fcrls            85  
Plp1          43217  
Apod          36194  
Klk6            287  
Gnb4           1071  
Cyp2d22        4060  
Slc2a4          782  
Asl            1170  
Gjc2            288  
Exd2           1619  

```

And the index columns do not need to have the same name. 

```python
print(annot2)
```

```output
                                                          description
external_gene_name                                                   
Slc2a4              solute carrier family 2 (facilitated glucose t...
Gnb4                guanine nucleotide binding protein (G protein)...
Fcrls               Fc receptor-like S, scavenger receptor [Source...
Gjc2                gap junction protein, gamma 2 [Source:MGI Symb...
Klk6                kallikrein related-peptidase 6 [Source:MGI Sym...
Plp1                proteolipid protein (myelin) 1 [Source:MGI Sym...
Cyp2d22             cytochrome P450, family 2, subfamily d, polype...
Exd2                exonuclease 3'-5' domain containing 2 [Source:...
Apod                apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...
Asl                 argininosuccinate lyase [Source:MGI Symbol;Acc...
```

```python
print(rnaseq_mini.join(annot2))
```

```output
             sample  expression  \
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

                                               description  
gene                                                        
Asl      argininosuccinate lyase [Source:MGI Symbol;Acc...  
Apod     apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...  
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...  
Klk6     kallikrein related-peptidase 6 [Source:MGI Sym...  
Fcrls    Fc receptor-like S, scavenger receptor [Source...  
Slc2a4   solute carrier family 2 (facilitated glucose t...  
Exd2     exonuclease 3'-5' domain containing 2 [Source:...  
Gjc2     gap junction protein, gamma 2 [Source:MGI Symb...  
Plp1     proteolipid protein (myelin) 1 [Source:MGI Sym...  
Gnb4     guanine nucleotide binding protein (G protein)...  
```

`join` by default uses the indices of the dataframes it combines. 
To change this, we can use the `on` parameter.
For instance, let's say we want to combine our sample metadata back with `rnaseq_mini`. 

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

```python
print(rnaseq_mini.join(metadata, on="sample"))
```

```output
             sample  expression      organism  age     sex   infection  \
gene                                                                     
Asl      GSM2545336        1170  Mus musculus    8  Female  InfluenzaA   
Apod     GSM2545336       36194  Mus musculus    8  Female  InfluenzaA   
Cyp2d22  GSM2545336        4060  Mus musculus    8  Female  InfluenzaA   
Klk6     GSM2545336         287  Mus musculus    8  Female  InfluenzaA   
Fcrls    GSM2545336          85  Mus musculus    8  Female  InfluenzaA   
Slc2a4   GSM2545336         782  Mus musculus    8  Female  InfluenzaA   
Exd2     GSM2545336        1619  Mus musculus    8  Female  InfluenzaA   
Gjc2     GSM2545336         288  Mus musculus    8  Female  InfluenzaA   
Plp1     GSM2545336       43217  Mus musculus    8  Female  InfluenzaA   
Gnb4     GSM2545336        1071  Mus musculus    8  Female  InfluenzaA   

          strain  time      tissue  mouse  
gene                                       
Asl      C57BL/6     8  Cerebellum     14  
Apod     C57BL/6     8  Cerebellum     14  
Cyp2d22  C57BL/6     8  Cerebellum     14  
Klk6     C57BL/6     8  Cerebellum     14  
Fcrls    C57BL/6     8  Cerebellum     14  
Slc2a4   C57BL/6     8  Cerebellum     14  
Exd2     C57BL/6     8  Cerebellum     14  
Gjc2     C57BL/6     8  Cerebellum     14  
Plp1     C57BL/6     8  Cerebellum     14  
Gnb4     C57BL/6     8  Cerebellum     14  
```

Note that if we want to join on columns between dataframes which have different names we can simply rename the columns we wish to join on. 
If there is a reason we don't want to do this, we can instead use the more powerful but harder to use pandas [merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html?highlight=merge#pandas.DataFrame.merge) function. 

### Missing and duplicate data

In the above join, there were a few other things going on. 
The first is that multiple rows of `rnaseq_mini` contain the same sample. 
Pandas by default will repeat the rows wherever needed when joining. 

The second is that the `metadata` dataframe contains samples which are not present in `rnaseq_mini`. 
By default, they were dropped from the dataframe.
However, there might be cases where we want to keep our data. 
Let's explore how joining handles missing data with another example:

```python
print(annot3)
```

```output
                                          gene_description
gene                                                      
mt-Rnr1  mitochondrially encoded 12S rRNA [Source:MGI S...
mt-Rnr2  mitochondrially encoded 16S rRNA [Source:MGI S...
Slc2a4   solute carrier family 2 (facilitated glucose t...
Asl      argininosuccinate lyase [Source:MGI Symbol;Acc...
Exd2     exonuclease 3'-5' domain containing 2 [Source:...
Gnb4     guanine nucleotide binding protein (G protein)...
mt-Tl1   mitochondrially encoded tRNA leucine 1 [Source...
Apod     apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...
mt-Tv    mitochondrially encoded tRNA valine [Source:MG...
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...
mt-Tf    mitochondrially encoded tRNA phenylalanine [So...
Fcrls    Fc receptor-like S, scavenger receptor [Source...
Gjc2     gap junction protein, gamma 2 [Source:MGI Symb...
Plp1     proteolipid protein (myelin) 1 [Source:MGI Sym...
```

::: challenge
## Missing data in joins

What data is missing between `annot3` and `rnaseq_mini`?

Try joining them, how is this missing data handled?

::: solution

There are both genes in `annot3` but not in `rnaseq_mini`, and genes in `rnaseq_mini` not in `annot3`.

When we join, we keep rows from rnaseq_mini with missing data and drop rows from `annot3` with missing data. 

```python
print(rnaseq_mini.join(annot3))
```

```output
             sample  expression  \
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

                                          gene_description  
gene                                                        
Asl      argininosuccinate lyase [Source:MGI Symbol;Acc...  
Apod     apolipoprotein D [Source:MGI Symbol;Acc:MGI:88...  
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...  
Klk6                                                   NaN  
Fcrls    Fc receptor-like S, scavenger receptor [Source...  
Slc2a4   solute carrier family 2 (facilitated glucose t...  
Exd2     exonuclease 3'-5' domain containing 2 [Source:...  
Gjc2     gap junction protein, gamma 2 [Source:MGI Symb...  
Plp1     proteolipid protein (myelin) 1 [Source:MGI Sym...  
Gnb4     guanine nucleotide binding protein (G protein)...  
```

:::
:::

### Types of joins

The reason we see the above behavior is because by default pandas performs a **left join**.



