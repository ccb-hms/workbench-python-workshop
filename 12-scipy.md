---
title: "Perform Statistical Tests with Scipy"
teaching: 7
exercises: 3
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do I use distributions?
- How do I perform statistical tests?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explore the `scipy` library.
- Define and sample from a distribution. 
- Perform a t-test on prepared data. 

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Scipy is a package with a variety of scientific computing functionality.
- Scipy.stats contains functionality for distributions and statistical tests. 

::::::::::::::::::::::::::::::::::::::::::::::::

## Sci-py is a package with a variety of scientific computing functionality. 

[SciPy](https://scipy.org/) contains a variety of scientific computing modules. 
It has modules for:

- Integration (scipy.integrate)
- Optimization (scipy.optimize)
- Interpolation (scipy.interpolate)
- Fourier Transforms (scipy.fft)
- Signal Processing (scipy.signal)
- Linear Algebra (scipy.linalg)
- Compressed Sparse Graph Routines (scipy.sparse.csgraph)
- Spatial data structures and algorithms (scipy.spatial)
- Statistics (scipy.stats)
- Multidimensional image processing (scipy.ndimage)

We are only going to look at some of the functionality in `scipy.stats`. 

Let's load the module and take a look. Note that the output is not shown here as it is very long. 

```python
import scipy.stats as stats

help(stats)
```

This is a lot, but we can see that `scipy.stats` has a variety of different distributions and functions available for performing statistics. 

## Sampling from a distribution

One common reason use a distribution is if we want to generate some random with some kind of structure. 
Python has a [built-in random module](https://docs.python.org/3/library/random.html) which can perform some basic tasks like getting uniform random numbers or shuffling a list, but we need more power to generate more complicated random data. 

Distributions in scipy share a set of methods. 
Some of the important ones are:

- `rvs` which generates random numbers from the distribution. 
- `pdf` which gets the probability density function. 
- `cdf` which gets the cumulative distribution function.
- `stats` which reports various properties of the distribution. 

Scipy's convention is not to create distribution objects, but to pass parameters as needed into each of these functions. 
For instance, to generate 100 normally distributed variables with a mean of 8 and a standard deviation of 3, we call `stats.norm.rvs` with those parameters as `loc` and `scale`, respectively. 

```python
stats.norm.rvs(size=100, loc=8, scale=3)
```

```output
array([10.81294892,  9.35960484,  4.13064284,  5.01349956, 11.91542804,
        4.40831262,  7.873177  ,  2.80427116,  6.93137287,  8.6260419 ,
       10.48824661,  4.03472414, 10.01449037,  7.37493941,  6.8729883 ,
        8.8247789 ,  6.9956787 ,  8.2084562 ,  9.4272925 ,  8.14680254,
        1.61100441,  7.14171227,  6.83756279, 13.15778935,  5.87752233,
       11.53910465,  9.7899608 , 10.99998659,  5.67069185,  4.43542582,
        8.05798467,  4.56883381, 11.2219477 ,  9.49666323,  6.09194101,
       10.0844057 , 10.3107259 ,  5.50683223,  9.97121225, 10.71650187,
        7.11465757,  1.81891326,  5.11893454,  5.7602409 ,  7.21754014,
        8.79988949, 10.37762164, 14.33689265,  6.33571171,  8.06869862,
        8.54040514,  7.70807529, 11.35719793,  9.60738274,  6.02998292,
        5.68116531,  2.35490176, 10.74582778,  9.8661685 , 13.39578467,
       10.04354226,  7.28494967, 10.16128058, -0.59049207,  7.2093563 ,
        6.81705905,  5.95187581,  7.51137727, 12.00011092, 10.99417942,
        7.84189409,  1.51154885,  5.85094646,  9.24591807, 10.0216898 ,
        9.79350275,  7.26730344,  4.94176518,  9.06454997,  2.99129021,
       10.8972046 , 12.51642136,  7.31422469,  4.54086114,  4.36204651,
        8.33272365,  9.53609612,  7.21441855,  8.58643188,  7.67419071,
       10.36948034,  4.405381  ,  8.16845496,  2.9752478 ,  5.93608394,
        4.91781677, 11.60177026,  7.97727669,  8.43215961,  6.97469055])
```

::: callout
### Numpy arrays

Note that the output isn't a list or pandas object, but is something called an `array`. 
This is an object in the [NumPy](https://numpy.org/) package. 
NumPy objects are used by most scientific packages in Python such as pandas, scikit-learn, and scipy. 
It provides a variety of objects and funcionality for performing fast numeric operations. 

Numpy arrays can be indexed and manipulated like Python lists in most instances, but provide extra functionality.
We will not be going into more detail about numpy during this workshop.
:::

## Performing a t-test with scipy 

First, let's load up a small toy dataset looking at how mouse weight responded to a particular treatment. 
This data contains 4 columns, the mouse label/number, whether or not it was treated, its sex and its weight. 

```python
import pandas as pd

url = "https://raw.githubusercontent.com/ccb-hms/workbench-python-workshop/main/episodes/data/mouse_weight_data.csv"
mouse_df = pd.read_csv(url, index_col="Mouse")
print(mouse_df)
```

```output
       Treated Sex  Weight
Mouse                     
1         True   M      22
2         True   F      25
3         True   M      27
4         True   F      23
5         True   M      23
6         True   F      24
7         True   M      26
8         True   F      24
9         True   M      27
10        True   F      27
11        True   M      25
12        True   F      27
13        True   M      26
14        True   F      24
15        True   M      26
16       False   F      25
17       False   M      29
18       False   F      28
19       False   M      27
20       False   F      28
21       False   M      25
22       False   F      23
23       False   M      24
24       False   F      30
25       False   M      24
26       False   F      30
27       False   M      30
28       False   F      28
29       False   M      27
30       False   F      24
```

We can start by seeing if the mean weight is different between the groups:

```python
treated_mean = mouse_df[mouse_df["Treated"]]["Weight"].mean()
untreated_mean = mouse_df[~mouse_df["Treated"]]["Weight"].mean()
print(f"Treated mean weight: {treated_mean:0.0f}g\nUntreated mean weight: {untreated_mean:0.0f}g")
```

```output
Treated mean weight: 25g
Untreated mean weight: 27g
```
We can see that there is a slight different, but it's unclear if it is real or just a source of randomnes in the data. 

::: callout
## The newline character

`\n` is a special character called the newline character. 
It tells Python to go to a new line of text. 
`\n` is univeral to almost all programming languages. 
:::

::: callout
## `~` in Pandas

Though before we saw that in Python `!` is how we indicate logical NOT, for pandas boolean series we have to use the `~` character to invert every boolean in the series. 
:::

Let's perform a t-test to check for a significant difference. 
A one-sample t-test is performed using `ttest_1samp`. 
As we want to compare two groups, we will need to use perform a two-sample t-test using `ttest_ind`.

```python
treated_weights = mouse_df[mouse_df["Treated"]]["Weight"]
untreated_weights = mouse_df[~mouse_df["Treated"]]["Weight"]

stats.ttest_ind(treated_weights, untreated_weights)
```
```output
Ttest_indResult(statistic=-2.2617859482443694, pvalue=0.03166586638057747)
```
Our p-value is about 0.03.

There are a number of arguments we could use to change the test, such as accounting for unequal variance and single-sides tests which can be found [here](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html)

::: challenge

Peform a t-test looking at whether there is a different by sex as opposed to treatment. 

::: solution

```python
m_weights = mouse_df[mouse_df["Sex"] == "M"]["Weight"]
f_weights = mouse_df[mouse_df["Sex"] == "F"]["Weight"]

stats.ttest_ind(m_weights, f_weights)
```
```output
Ttest_indResult(statistic=-0.16005488537123244, pvalue=0.8739869209422747)
```

:::
:::

