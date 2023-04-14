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

::: callout
## TODO 

Add this file to our repo as well so we're not relying on this location.
:::

```python
import pandas as pd

url = "https://github.com/carpentries-incubator/bioc-intro/raw/main/episodes/data/rnaseq.csv"
rnaseq_df = pd.read_csv(url)
print(rnaseq_df)
```

```output
gene      sample  expression      organism  age     sex   infection  \
0          Asl  GSM2545336        1170  Mus musculus    8  Female  InfluenzaA   
1         Apod  GSM2545336       36194  Mus musculus    8  Female  InfluenzaA   
2      Cyp2d22  GSM2545336        4060  Mus musculus    8  Female  InfluenzaA   
3         Klk6  GSM2545336         287  Mus musculus    8  Female  InfluenzaA   
4        Fcrls  GSM2545336          85  Mus musculus    8  Female  InfluenzaA   
...        ...         ...         ...           ...  ...     ...         ...   
32423    Mgst3  GSM2545380        2151  Mus musculus    8  Female  InfluenzaA   
32424   Lrrc52  GSM2545380           5  Mus musculus    8  Female  InfluenzaA   
32425     Rxrg  GSM2545380          49  Mus musculus    8  Female  InfluenzaA   
32426    Lmx1a  GSM2545380          72  Mus musculus    8  Female  InfluenzaA   
32427     Pbx1  GSM2545380        4795  Mus musculus    8  Female  InfluenzaA   

        strain  time      tissue  mouse  ENTREZID  \
0      C57BL/6     8  Cerebellum     14    109900   
1      C57BL/6     8  Cerebellum     14     11815   
2      C57BL/6     8  Cerebellum     14     56448   
3      C57BL/6     8  Cerebellum     14     19144   
4      C57BL/6     8  Cerebellum     14     80891   
...        ...   ...         ...    ...       ...   
32423  C57BL/6     8  Cerebellum     19     66447   
32424  C57BL/6     8  Cerebellum     19    240899   
32425  C57BL/6     8  Cerebellum     19     20183   
32426  C57BL/6     8  Cerebellum     19    110648   
32427  C57BL/6     8  Cerebellum     19     18514   

                                                 product     ensembl_gene_id  \
0         argininosuccinate lyase, transcript variant X1  ENSMUSG00000025533   
1                 apolipoprotein D, transcript variant 3  ENSMUSG00000022548   
2      cytochrome P450, family 2, subfamily d, polype...  ENSMUSG00000061740   
3      kallikrein related-peptidase 6, transcript var...  ENSMUSG00000050063   
4      Fc receptor-like S, scavenger receptor, transc...  ENSMUSG00000015852   
...                                                  ...                 ...   
32423             microsomal glutathione S-transferase 3  ENSMUSG00000026688   
32424                  leucine rich repeat containing 52  ENSMUSG00000040485   
32425    retinoid X receptor gamma, transcript variant 1  ENSMUSG00000015843   
32426  LIM homeobox transcription factor 1 alpha, tra...  ENSMUSG00000026686   
32427  pre B cell leukemia homeobox 1, transcript var...  ENSMUSG00000052534   

      external_synonym chromosome_name    gene_biotype  \
0        2510006M18Rik               5  protein_coding   
1                  NaN              16  protein_coding   
2                 2D22              15  protein_coding   
3                 Bssp               7  protein_coding   
4        2810439C17Rik               3  protein_coding   
...                ...             ...             ...   
32423    2010012L10Rik               1  protein_coding   
32424    4930413P14Rik               1  protein_coding   
32425            Nr2b3               1  protein_coding   
32426           Lmx1.1               1  protein_coding   
32427    2310056B04Rik               1  protein_coding   

                                   phenotype_description  \
0                  abnormal circulating amino acid level   
1                             abnormal lipid homeostasis   
2                               abnormal skin morphology   
3                                abnormal cytokine level   
4        decreased CD8-positive alpha-beta T cell number   
...                                                  ...   
32423                  decreased mean corpuscular volume   
32424                          abnormal sperm physiology   
32425                       abnormal bone mineralization   
32426                            abnormal bony labyrinth   
32427  abnormal adrenal gland zona fasciculata morpho...   

      hsapiens_homolog_associated_gene_name  
0                                       ASL  
1                                      APOD  
2                                    CYP2D6  
3                                      KLK6  
4                                     FCRL2  
...                                     ...  
32423                                 MGST3  
32424                                LRRC52  
32425                                  RXRG  
32426                                 LMX1A  
32427                                  PBX1  

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
             sample  expression      organism  age     sex   infection  \
gene                                                                     
Asl      GSM2545336        1170  Mus musculus    8  Female  InfluenzaA   
Apod     GSM2545336       36194  Mus musculus    8  Female  InfluenzaA   
Cyp2d22  GSM2545336        4060  Mus musculus    8  Female  InfluenzaA   
Klk6     GSM2545336         287  Mus musculus    8  Female  InfluenzaA   
Fcrls    GSM2545336          85  Mus musculus    8  Female  InfluenzaA   
...             ...         ...           ...  ...     ...         ...   
Mgst3    GSM2545380        2151  Mus musculus    8  Female  InfluenzaA   
Lrrc52   GSM2545380           5  Mus musculus    8  Female  InfluenzaA   
Rxrg     GSM2545380          49  Mus musculus    8  Female  InfluenzaA   
Lmx1a    GSM2545380          72  Mus musculus    8  Female  InfluenzaA   
Pbx1     GSM2545380        4795  Mus musculus    8  Female  InfluenzaA   

          strain  time      tissue  mouse  ENTREZID  \
gene                                                  
Asl      C57BL/6     8  Cerebellum     14    109900   
Apod     C57BL/6     8  Cerebellum     14     11815   
Cyp2d22  C57BL/6     8  Cerebellum     14     56448   
Klk6     C57BL/6     8  Cerebellum     14     19144   
Fcrls    C57BL/6     8  Cerebellum     14     80891   
...          ...   ...         ...    ...       ...   
Mgst3    C57BL/6     8  Cerebellum     19     66447   
Lrrc52   C57BL/6     8  Cerebellum     19    240899   
Rxrg     C57BL/6     8  Cerebellum     19     20183   
Lmx1a    C57BL/6     8  Cerebellum     19    110648   
Pbx1     C57BL/6     8  Cerebellum     19     18514   

                                                   product  \
gene                                                         
Asl         argininosuccinate lyase, transcript variant X1   
Apod                apolipoprotein D, transcript variant 3   
Cyp2d22  cytochrome P450, family 2, subfamily d, polype...   
Klk6     kallikrein related-peptidase 6, transcript var...   
Fcrls    Fc receptor-like S, scavenger receptor, transc...   
...                                                    ...   
Mgst3               microsomal glutathione S-transferase 3   
Lrrc52                   leucine rich repeat containing 52   
Rxrg       retinoid X receptor gamma, transcript variant 1   
Lmx1a    LIM homeobox transcription factor 1 alpha, tra...   
Pbx1     pre B cell leukemia homeobox 1, transcript var...   

            ensembl_gene_id external_synonym chromosome_name    gene_biotype  \
gene                                                                           
Asl      ENSMUSG00000025533    2510006M18Rik               5  protein_coding   
Apod     ENSMUSG00000022548              NaN              16  protein_coding   
Cyp2d22  ENSMUSG00000061740             2D22              15  protein_coding   
Klk6     ENSMUSG00000050063             Bssp               7  protein_coding   
Fcrls    ENSMUSG00000015852    2810439C17Rik               3  protein_coding   
...                     ...              ...             ...             ...   
Mgst3    ENSMUSG00000026688    2010012L10Rik               1  protein_coding   
Lrrc52   ENSMUSG00000040485    4930413P14Rik               1  protein_coding   
Rxrg     ENSMUSG00000015843            Nr2b3               1  protein_coding   
Lmx1a    ENSMUSG00000026686           Lmx1.1               1  protein_coding   
Pbx1     ENSMUSG00000052534    2310056B04Rik               1  protein_coding   

                                     phenotype_description  \
gene                                                         
Asl                  abnormal circulating amino acid level   
Apod                            abnormal lipid homeostasis   
Cyp2d22                           abnormal skin morphology   
Klk6                               abnormal cytokine level   
Fcrls      decreased CD8-positive alpha-beta T cell number   
...                                                    ...   
Mgst3                    decreased mean corpuscular volume   
Lrrc52                           abnormal sperm physiology   
Rxrg                          abnormal bone mineralization   
Lmx1a                              abnormal bony labyrinth   
Pbx1     abnormal adrenal gland zona fasciculata morpho...   

        hsapiens_homolog_associated_gene_name  
gene                                           
Asl                                       ASL  
Apod                                     APOD  
Cyp2d22                                CYP2D6  
Klk6                                     KLK6  
Fcrls                                   FCRL2  
...                                       ...  
Mgst3                                   MGST3  
Lrrc52                                 LRRC52  
Rxrg                                     RXRG  
Lmx1a                                   LMX1A  
Pbx1                                     PBX1  

[32428 rows x 18 columns]

```


## Use the `DataFrame.info()` method to find out more about a dataframe.

```python
rnaseq_df.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
Index: 32428 entries, Asl to Pbx1
Data columns (total 18 columns):
 #   Column                                 Non-Null Count  Dtype 
---  ------                                 --------------  ----- 
 0   sample                                 32428 non-null  object
 1   expression                             32428 non-null  int64 
 2   organism                               32428 non-null  object
 3   age                                    32428 non-null  int64 
 4   sex                                    32428 non-null  object
 5   infection                              32428 non-null  object
 6   strain                                 32428 non-null  object
 7   time                                   32428 non-null  int64 
 8   tissue                                 32428 non-null  object
 9   mouse                                  32428 non-null  int64 
 10  ENTREZID                               32428 non-null  int64 
 11  product                                31240 non-null  object
 12  ensembl_gene_id                        32428 non-null  object
 13  external_synonym                       26620 non-null  object
 14  chromosome_name                        32428 non-null  object
 15  gene_biotype                           32428 non-null  object
 16  phenotype_description                  21912 non-null  object
 17  hsapiens_homolog_associated_gene_name  28138 non-null  object
dtypes: int64(5), object(13)
memory usage: 4.7+ MB

```

*   This is a `DataFrame`
*   32428
*   Twelve columns, each of which has two actual 64-bit floating point values.
    *   We will talk later about null values, which are used to represent missing observations.
*   Uses 208 bytes of memory.

## The `DataFrame.columns` variable stores information about the dataframe's columns.

*   Note that this is data, *not* a method.  (It doesn't have parentheses.)
    *   Like `math.pi`.
    *   So do not use `()` to try to call it.
*   Called a *member variable*, or just *member*.

```python
print(data.columns)
```

```output
Index(['gdpPercap_1952', 'gdpPercap_1957', 'gdpPercap_1962', 'gdpPercap_1967',
       'gdpPercap_1972', 'gdpPercap_1977', 'gdpPercap_1982', 'gdpPercap_1987',
       'gdpPercap_1992', 'gdpPercap_1997', 'gdpPercap_2002', 'gdpPercap_2007'],
      dtype='object')
```


## Use `DataFrame.T` to transpose a dataframe.

*   Sometimes want to treat columns as rows and vice versa.
*   Transpose (written `.T`) doesn't copy the data, just changes the program's view of it.
*   Like `columns`, it is a member variable.

```python
print(data.T)
```

```output
country           Australia  New Zealand
gdpPercap_1952  10039.59564  10556.57566
gdpPercap_1957  10949.64959  12247.39532
gdpPercap_1962  12217.22686  13175.67800
gdpPercap_1967  14526.12465  14463.91893
gdpPercap_1972  16788.62948  16046.03728
gdpPercap_1977  18334.19751  16233.71770
gdpPercap_1982  19477.00928  17632.41040
gdpPercap_1987  21888.88903  19007.19129
gdpPercap_1992  23424.76683  18363.32494
gdpPercap_1997  26997.93657  21050.41377
gdpPercap_2002  30687.75473  23189.80135
gdpPercap_2007  34435.36744  25185.00911
```

## Use `DataFrame.describe()` to get summary statistics about data.

`DataFrame.describe()` gets the summary statistics of only the columns that have numerical data. 
All other columns are ignored, unless you use the argument `include='all'`.
```python
print(data.describe())
```

```output
       gdpPercap_1952  gdpPercap_1957  gdpPercap_1962  gdpPercap_1967  \
count        2.000000        2.000000        2.000000        2.000000
mean     10298.085650    11598.522455    12696.452430    14495.021790
std        365.560078      917.644806      677.727301       43.986086
min      10039.595640    10949.649590    12217.226860    14463.918930
25%      10168.840645    11274.086022    12456.839645    14479.470360
50%      10298.085650    11598.522455    12696.452430    14495.021790
75%      10427.330655    11922.958888    12936.065215    14510.573220
max      10556.575660    12247.395320    13175.678000    14526.124650

       gdpPercap_1972  gdpPercap_1977  gdpPercap_1982  gdpPercap_1987  \
count         2.00000        2.000000        2.000000        2.000000
mean      16417.33338    17283.957605    18554.709840    20448.040160
std         525.09198     1485.263517     1304.328377     2037.668013
min       16046.03728    16233.717700    17632.410400    19007.191290
25%       16231.68533    16758.837652    18093.560120    19727.615725
50%       16417.33338    17283.957605    18554.709840    20448.040160
75%       16602.98143    17809.077557    19015.859560    21168.464595
max       16788.62948    18334.197510    19477.009280    21888.889030

       gdpPercap_1992  gdpPercap_1997  gdpPercap_2002  gdpPercap_2007
count        2.000000        2.000000        2.000000        2.000000
mean     20894.045885    24024.175170    26938.778040    29810.188275
std       3578.979883     4205.533703     5301.853680     6540.991104
min      18363.324940    21050.413770    23189.801350    25185.009110
25%      19628.685413    22537.294470    25064.289695    27497.598692
50%      20894.045885    24024.175170    26938.778040    29810.188275
75%      22159.406358    25511.055870    28813.266385    32122.777857
max      23424.766830    26997.936570    30687.754730    34435.367440
```

*   Not particularly useful with just two records,
    but very helpful when there are thousands.

::: challenge
## Reading Other Data

Read the data in `gapminder_gdp_americas.csv`
(which should be in the same directory as `gapminder_gdp_oceania.csv`)
into a variable called `americas`
and display its summary statistics.

::: solution
To read in a CSV, we use `pd.read_csv` and pass the filename `'data/gapminder_gdp_americas.csv'` to it.
We also once again pass the column name `'country'` to the parameter `index_col` in order to index by country.
The summary statistics can be displayed with the `DataFrame.describe()` method.
```python
americas = pd.read_csv('data/gapminder_gdp_americas.csv', index_col='country')
americas.describe()
```
:::
:::

::: challenge
## Inspecting Data

After reading the data for the Americas,
use `help(americas.head)` and `help(americas.tail)`
to find out what `DataFrame.head` and `DataFrame.tail` do.

1.  What method call will display the first three rows of this data?
2.  What method call will display the last three columns of this data?
    (Hint: you may need to change your view of the data.)

::: solution
1. We can check out the first five rows of `americas` by executing `americas.head()`
   (allowing us to view the head of the DataFrame). We can specify the number of rows we wish
   to see by specifying the parameter `n` in our call
   to `americas.head()`. To view the first three rows, execute:

   ```python
   americas.head(n=3)
   ```

   ```output
             continent  gdpPercap_1952  gdpPercap_1957  gdpPercap_1962  \
   country
   Argentina  Americas     5911.315053     6856.856212     7133.166023
   Bolivia    Americas     2677.326347     2127.686326     2180.972546
   Brazil     Americas     2108.944355     2487.365989     3336.585802

              gdpPercap_1967  gdpPercap_1972  gdpPercap_1977  gdpPercap_1982  \
   country
   Argentina     8052.953021     9443.038526    10079.026740     8997.897412
   Bolivia       2586.886053     2980.331339     3548.097832     3156.510452
   Brazil        3429.864357     4985.711467     6660.118654     7030.835878

              gdpPercap_1987  gdpPercap_1992  gdpPercap_1997  gdpPercap_2002  \
   country
   Argentina     9139.671389     9308.418710    10967.281950     8797.640716
   Bolivia       2753.691490     2961.699694     3326.143191     3413.262690
   Brazil        7807.095818     6950.283021     7957.980824     8131.212843

              gdpPercap_2007
   country
   Argentina    12779.379640
   Bolivia       3822.137084
   Brazil        9065.800825
   ```

2. To check out the last three rows of `americas`, we would use the command,
   `americas.tail(n=3)`, analogous to `head()` used above. However, here we want to look at
   the last three columns so we need to change our view and then use `tail()`. To do so, we
    create a new DataFrame in which rows and columns are switched:

   ```python
   americas_flipped = americas.T
   ```

   We can then view the last three columns of `americas` by viewing the last three rows
   of `americas_flipped`:
   ```python
   americas_flipped.tail(n=3)
   ```

   ```output
   country        Argentina  Bolivia   Brazil   Canada    Chile Colombia  \
   gdpPercap_1997   10967.3  3326.14  7957.98  28954.9  10118.1  6117.36
   gdpPercap_2002   8797.64  3413.26  8131.21    33329  10778.8  5755.26
   gdpPercap_2007   12779.4  3822.14   9065.8  36319.2  13171.6  7006.58

   country        Costa Rica     Cuba Dominican Republic  Ecuador    ...     \
   gdpPercap_1997    6677.05  5431.99             3614.1  7429.46    ...
   gdpPercap_2002    7723.45  6340.65            4563.81  5773.04    ...
   gdpPercap_2007    9645.06   8948.1            6025.37  6873.26    ...

   country          Mexico Nicaragua   Panama Paraguay     Peru Puerto Rico  \
   gdpPercap_1997   9767.3   2253.02  7113.69   4247.4  5838.35     16999.4
   gdpPercap_2002  10742.4   2474.55  7356.03  3783.67  5909.02     18855.6
   gdpPercap_2007  11977.6   2749.32  9809.19  4172.84  7408.91     19328.7

   country        Trinidad and Tobago United States  Uruguay Venezuela
   gdpPercap_1997             8792.57       35767.4  9230.24   10165.5
   gdpPercap_2002             11460.6       39097.1     7727   8605.05
   gdpPercap_2007             18008.5       42951.7  10611.5   11415.8
   ```

   
   This shows the data that we want, but we may prefer to display three columns instead of three rows,
   so we can flip it back:
   ```python
   americas_flipped.tail(n=3).T    
   ```
   
   __Note:__ we could have done the above in a single line of code by 'chaining' the commands:
   ```python
   americas.T.tail(n=3).T
   ```

:::
:::

::: challenge
## Reading Files in Other Directories

The data for your current project is stored in a file called `microbes.csv`,
which is located in a folder called `field_data`.
You are doing analysis in a notebook called `analysis.ipynb`
in a sibling folder called `thesis`:

```output
your_home_directory
+-- field_data/
|   +-- microbes.csv
+-- thesis/
    +-- analysis.ipynb
```

What value(s) should you pass to `read_csv` to read `microbes.csv` in `analysis.ipynb`?

::: solution
We need to specify the path to the file of interest in the call to `pd.read_csv`. We first need to 'jump' out of
the folder `thesis` using '../' and then into the folder `field_data` using 'field_data/'. Then we can specify the filename `microbes.csv.
The result is as follows:
```python
data_microbes = pd.read_csv('../field_data/microbes.csv')
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
In order to write the DataFrame `americas` to a file called `processed.csv`, execute the following command:
```python
americas.to_csv('processed.csv')
```

For help on `to_csv`, you could execute, for example:
```python
help(americas.to_csv)
```

Note that `help(to_csv)` throws an error! This is a subtlety and is due to the fact that `to_csv` is NOT a function in 
and of itself and the actual call is `americas.to_csv`. 
:::
:::
