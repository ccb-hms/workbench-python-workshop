---
title: "String Manipulation"
teaching: 10
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I manipulate text?
- How can I create neat and dynamic text output?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Extract substrings of interest.
- Format dynamic strings using f-strings.
- Explore Python's built-in string functions

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Strings can be indexed and sliced.
- Strings cannot be directly altered.
- You can build complex strings based on other variables using f-strings and format.
- Python has a variety of useful built-in string functions.
::::::::::::::::::::::::::::::::::::::::::::::::

## Strings can be indexed and sliced.

As we saw in class during the [types](03-types.md) lesson, strings in Python can be **indexed** and **sliced**

```python
my_string = "What a lovely day."

# Indexing
my_string[2]
```

```output
'a'
```

```python
# Slicing
my_string[1:3]
```

```output
'ha'
```

```python
my_string[:3] # Same as my_string[0:3]
```

```output
'Wha'
```

```python
my_string[1:] # Same as my_string[1:len(my_string)]
```

```output
'hat a lovely day.'
```
## Strings are immutable.

We will talk about this concept in more detail when we explore [lists](05-lists.md).
However, for now it is important to note that strings, like simple types, *cannot have their values be altered* in Python. 
Instead, a new value is created. 

For simple types, this behavior isn't that noticable:

```python
x = 10
# While this line appears to be changing the value 10 to 11, in reality a new integer with the value 11 is created and assigned to x. 
x = x + 1
x
```

```output
11
```

However, for strings we can easily cause errors if we attempt to change them directly:

```python
# This attempts to change the last character to 'g' 
my_string[-1] = "g"
```

```error
---------------------------------------------------------------------
TypeError                           Traceback (most recent call last)
Input In [6], in <cell line: 1>()
----> 1 my_string[-1] = "g"

TypeError: 'str' object does not support item assignment
```

Thus, we need to learn ways in Python to manipulate and build strings. 
In this instance, we can build a new string using indexing:

```python
my_new_string = my_string[:-2] + "g"
my_new_string
```

```output
'What a lovely dag'
```

Or use the built-in function `str.replace()` if we wanted to replace a larger portion of the string.

```python
my_new_string = my_string.replace("day", 'dog')
print(my_new_string)
```

```output
What a lovely dog.
```

## Build complex strings based on other variables using format.

What if we want to use values inside of a string?
For instance, say we want to print a sentence denoting how many and the percent of samples we dropped and kept as a part of a quality control analysis. 

Suppose we have variables denoting how many samples and dropped samples we have:

```python
good_samples = 964
bad_samples = 117
percent_dropped = bad_samples/(good_samples + bad_samples)
```

One option would be to simply put everything in `print`:

```python
print("Dropped", percent_dropped, "percent samples")
```

```output
Dropped 0.10823311748381129 percent samples
```

Or we could convert and use addition:

```python
out_str = "Dropped " + str(percent_dropped) + "% samples"
print(out_str)
```

```output
Dropped 0.10823311748381129% samples
```

However, both of these options don't give us as much control over how the percent is displayed. 
Python uses systems called **string formatting** and **f-strings** to give us greater control. 

We can use Python's built-in `format` function to create better-looking text:

```python
print('Dropped {0:.2%} of samples, with {1:n} samples remaining.'.format(percent_dropped, good_samples))
```

```output
Dropped 10.82% of samples, with 964 samples remaining.
```

Calls to format have a number of components:

- The use of brackets `{}` to create *replacement fields*.
- The index inside each bracket (0 and 1) to denote the index of the variable to use. 
- Format instructions. `.2%` indicates that we want to format the number as a percent with 2 decimal places. `n` indicates that we want to format as a number. 
- `format` we call format on the string, and as arguments give it the variables we want to use. The order of variables here are the indices referenced in replacement fields. 

For instance, we can switch or repeat indices:

```python
print('Dropped {1:.2%} of samples, with {0:n} samples remaining.'.format(percent_dropped, good_samples))
print('Dropped {0:.2%} of samples, with {0:n} samples remaining.'.format(percent_dropped, good_samples))
```

```output
Dropped 96400.00% of samples, with 964 samples remaining.
Dropped 10.82% of samples, with 0.108233 samples remaining.
```

Python has a shorthand for using format called `f-strings`. 
These strings let us directly use variables and create expressions inside of strings. 
We denote an `f-string` by putting `f` in front of the string definition:

```python
print(f'Dropped {percent_dropped:.2%} of samples, with {good_samples:n} samples remaining.')
print(f'Dropped {100*(bad_samples/(good_samples + bad_samples)):.2f}% of samples, with {good_samples:n} samples remaining.')
```

```output
Dropped 10.82% of samples, with 964 samples remaining.
Dropped 10.82% of samples, with 964 samples remaining.
```

Here, `{100*(bad_samples/(good_samples + bad_samples)):.2f}` is treated as an expression, and then printed as a **f**loat with 2 digits. 

Full documenation on all of the string formatting mini-language can be found [here](https://docs.python.org/3/library/string.html#string-formatting).


## Python has many useful built-in functions for string manipulation.

Python has many built-in methods for manipulating strings; simple and powerful text manipulation is considered one of Python's strengths.
We will go over some of the more common and useful functions here, but be aware that there are many more you can find in the [official documentation](https://docs.python.org/3/library/string.html).

### Dealing with whitespace

`str.strip()` strips the whitespace from the beginning and ending of a string. 
This is especially useful when reading in files which might have hidden spaces or tabs at the end of lines. 

```python
"  a   \t\t".strip()
```
Note that `\t` denotes a tab character. 

```output
'a'
```

`str.split()` strips a string up into a list of strings by some character. 
By default it uses whitespace, but we can give any set character to split by. 
We will learn how to use lists in the next session. 

```python
"My favorite sentence I hope nothing bad happens to it".split()
```

```output
['My',
 'favorite',
 'sentence',
 'I',
 'hope',
 'nothing',
 'bad',
 'happens',
 'to',
 'it']
 ```

 ```python
 "2023-:04-:12".split('-:')
 ```

 ```output
 ['2023', '04', '12']
 ```

### Pattern matching

`str.find()` find the first occurrence of the specified string inside the search string, and `str.rfind()` finds the last. 

```python
"(collected)ENSG00000010404(inprogress)ENSG000000108888".find('ENSG')
```

```output
11
```

```python
"(collected)ENSG00000010404(inprogress)ENSG000000108888".rfind('ENSG')
```

```output
38
```

`str.startswith()` and `str.endswith()` perform a simlar function but return a `bool` based on whether or not the string starts or ends with a particular string. 

```python
"(collected)ENSG00000010404".startswith('ENSG')
```

```output
False
```

```python
"ENSG00000010404".startswith('ENSG')
```

```output
True
```

### Case

`str.upper()`, `str.lower()`, `str.capitalize()`, and `str.title()` all change the capitalization of strings. 

```python
my_string = "liters (coffee) per minute"
print(my_string.lower())
print(my_string.upper())
print(my_string.capitalize())
print(my_string.title())
```

```output
liters (coffee) per minute
LITERS (COFFEE) PER MINUTE
Liters (coffee) per minute
Liters (Coffee) Per Minute
```

::: challenge

A common problem when analyzing data is that multiple features of the data will be stored as a column name or single string. 

For instance, consider the following column headers:

```
WT_0Min_1	WT_0Min_2	X.RAB7A_0Min_1	X.RAB7A_0Min_2	WT_5Min_1	WT_5Min_2	X.RAB7A_5Min_1	X.RAB7A_5Min_2	X.NPC1_5Min_1	X.NPC1_5Min_2
```

There are two variables of interest, the time, 0, 5, or 60 minutes post-infection, and the genotype, WT, NPC1 knockout and RAB7A knockout. We also have replicate information at the end of each column header. 
For now, let's just try extracting the timepoint, genotype, and replicate from a single column header. 

Given the string:
```python
sample_info = "X.RAB7A_0Min_1"
```

Try to print the string:

```
Sample is 0min RABA7A knockout, replicate 1. 
```

Using f-strings, slicing and indexing, and built-in string functions. 
You can try to use lists as a challenge, but its fine to instead get each piece of information separately from `sample_info`. 
:::
