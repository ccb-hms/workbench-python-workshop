---
title: "Basic Types"
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- What kinds of data are there in Python?
- How are different data types treated differently?
- How can I identify a variable's type?
- How can I convert between types?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain key differences between integers and floating point numbers.
- Explain key differences between numbers and character strings.
- Use built-in functions to convert between integers, floating point numbers, and strings.
- Subset a string using slicing. 

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Every value has a type.
- Use the built-in function `type` to find the type of a value.
- Types control what operations can be done on values.
- Strings can be added and multiplied.
- Strings have a length (but numbers don't).

::::::::::::::::::::::::::::::::::::::::::::::::

## Every value has a type.

*   Every value in a program has a specific type.
*   Integer (`int`): represents positive or negative whole numbers like 3 or -512.
*   Floating point number (`float`): represents real numbers like 3.14159 or -2.5.
*   Character string (usually called "string", `str`): text.
    *   Written in either single quotes or double quotes (as long as they match).
    *   The quote marks aren't printed when the string is displayed.

## Use the built-in function `type` to find the type of a value.

*   Use the built-in function `type` to find out what type a value has.
*   Works on variables as well.
    *   But remember: the *value* has the type --- the *variable* is just a label.

```python
print(type(52))
```

```output
<class 'int'>
```

```python
fitness = 'average'
print(type(fitness))
```

```output
<class 'str'>
```
## Types control what operations (or methods) can be performed on a given value.

*   A value's type determines what the program can do to it.

```python
print(5 - 3)
```

```output
2
```


```python
print('hello' - 'h')
```

```output
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-2-67f5626a1e07> in <module>()
----> 1 print('hello' - 'h')

TypeError: unsupported operand type(s) for -: 'str' and 'str'
```
## Simple and compound types.

We can broadly split different types into two categories in Python: *simple* types and *compound* types. 

Simple types consist of a single value. 
In Python, these simple types include:

- `int`
- `float`
- `bool`
- `NoneType`

Compound types contain **multiple** values. 
Compound types include:

- `str`
- `list`
- `dictionary`
- `tuple`
- `set`

In this lesson we will be learning about simple types and strings (`str`), which are made up of multiple characters. 
We will go into more detail on other compound types in future lessons. 

## You can use the "+" and "*" operators on strings.

*   "Adding" character strings concatenates them.

```python
full_name = 'Ahmed' + ' ' + 'Walsh'
print(full_name)
```

```output
Ahmed Walsh
```

::: callout
## The empty string

We can initialize a string to contain no letter, `empty = ""`.
This is called the **empty string**, and is often used when we want to build up a string character by character
:::


*   Multiplying a character string by an integer _N_ creates a new string that consists of that character string repeated  _N_ times.
    *   Since multiplication is repeated addition.

```python
separator = '=' * 10
print(separator)
```

```output
==========
```


## Strings have a length (but numbers don't).

*   The built-in function `len` counts the number of characters in a string.

```python
print(len(full_name))
```

```output
11
```


*   But numbers don't have a length (not even zero).

```python
print(len(52))
```

```output
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-3-f769e8e8097d> in <module>()
----> 1 print(len(52))

TypeError: object of type 'int' has no len()
```

## You must convert numbers to strings or vice versa when operating on them.

*   Cannot add numbers and strings.

```python
print(1 + '2')
```

```error
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-4-fe4f54a023c6> in <module>()
----> 1 print(1 + '2')

TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

*   The result here would be `3` if both values were considered `int`s, and `'12'` if both values were considered strings. Due to this ambiguity addition is not allowed between strings and integers.
*   Some types can be converted to other types by using the type name as a function.

```python
print(1 + int('2'))
print(str(1) + '2')
```

```output
3
12
```


## You can mix integers and floats freely in operations.

*   Integers and floating-point numbers can be mixed in arithmetic.
    *   Python 3 automatically converts integers to floats as needed.

```python
print('half is', 1 / 2.0)
print('three squared is', 3.0 ** 2)
```

```output
half is 0.5
three squared is 9.0
```


## Variables only change value when something is assigned to them.

*   If we make one cell in a spreadsheet depend on another,
    and update the latter,
    the former updates automatically.
*   This does **not** happen in programming languages.

```python
variable_one = 1
variable_two = 5 * variable_one
variable_one = 2
print('first is', variable_one, 'and second is', variable_two)
```

```output
first is 2 and second is 5
```


*   The computer reads the value of `variable_one` when doing the multiplication,
    creates a new value, and assigns it to `variable_two`.
*   Afterwards, the value of `variable_two` is set to the new value and *not dependent on `variable_one`* so its value
    does not automatically change when `variable_one` changes.

::: challenge

## Fractions

 What type of value is 3.4?
 How can you find out?

::: solution
 
  It is a floating-point number (often abbreviated "float").
  It is possible to find out by using the built-in function `type()`.
 
  ```python
  print(type(3.4))
  ```

  ```output
  <class 'float'>
  ```
:::
:::

::: challenge
 ## Automatic Type Conversion

 What type of value is 3.25 + 4?

::: solution
 
It is a float:
integers are automatically converted to floats as necessary.

```python
result = 3.25 + 4
print(result, 'is', type(result))
```

```output
7.25 is <class 'float'>
```
:::
:::

::: challenge
## Choose a Type

 What type of value (integer, floating point number, or character string)
 would you use to represent each of the following?  Try to come up with more than one good answer for each problem.  For example, in  # 1, when would counting days with a floating point variable make more sense than using an integer?  

 1. Number of days since the start of the year.
 2. Time elapsed from the start of the year until now in days.
 3. Serial number of a piece of lab equipment.
 4. A lab specimen's age
 5. Current population of a city.
 6. Average population of a city over time.

::: solution
 
  The answers to the questions are:
  1. Integer, since the number of days would lie between 1 and 365. 
  2. Floating point, since fractional days are required
  3. Character string if serial number contains letters and numbers, otherwise integer if the serial number consists only of numerals
  4. This will vary! How do you define a specimen's age? whole days since collection (integer)? date and time (string)?
  5. Choose floating point to represent population as large aggregates (eg millions), or integer to represent population in units of individuals.
  6. Floating point number, since an average is likely to have a fractional part.
:::
:::

::: challenge
## Division Types

 In Python 3, the `//` operator performs integer (whole-number) floor division, the `/` operator performs floating-point
 division, and the `%` (or *modulo*) operator calculates and returns the remainder from integer division:

 ```python
 print('5 // 3:', 5 // 3)
 print('5 / 3:', 5 / 3)
 print('5 % 3:', 5 % 3)
 ```


 ```output
 5 // 3: 1
 5 / 3: 1.6666666666666667
 5 % 3: 2
 ```

Imagine that you are buying cages for mice. 
`num_mice` is the number of mice you need cages for,
and `num_per_cage` is the maximum number of mice which can live in a single cage.
Write an expression that calculates the exact number of cages you need to purchase. 

 ```python
num_mice = 56
num_per_cage = 3
```

::: solution
  We want the number of cages to house all of our mice, which is the rounded up result of `num_mice / num_per_cage`. This is 
  equivalent to performing a floor division with `//` and adding 1. Before
  the division we need to subtract 1 from the number of mice to deal with 
  the case where `num_mice` is evenly divisible by `num_per_cage`.

```python
num_cages = ((num_mice - 1) // num_per_cage) + 1

print(num_mice, 'mice,', num_per_cage, 'per cage:', num_cages, 'cages')
```

```output
56 mice, 3 per cage: 19 cages
```
:::
:::

## Strings to Numbers
Where reasonable, `float()` will convert a string to a floating point number,
and `int()` will convert a floating point number to an integer:

```python
print("string to float:", float("3.6"))
print("float to int:", int(3.6))
```

```output
string to float: 3.4
float to int: 3
```
Note that converting a `float` to an `int` does not round the result, but instead truncates it by removing everything past the decimal point. 

If the conversion doesn't make sense, however, an error message will occur.

```python
print("string to float:", float("Hello world!"))
```


```error
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-5-df3b790bf0a2> in <module>
----> 1 print("string to float:", float("Hello world!"))
 ValueError: could not convert string to float: 'Hello world!'
```

::: challenge
What do you expect the following program to do?

What does it actually do?

Why do you think it does that?

```python
print("fractional string to int:", int("3.6"))
```
 
::: solution
What do you expect this program to do? It would not be so unreasonable to expect the Python 3 `int` command to
convert the string "3.4" to 3.4 and an additional type conversion to 3. After all, Python 3 performs a lot of other
magic - isn't that part of its charm?
 
```python
int("3.6")
```

```error
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-2-ec6729dfccdc> in <module>
----> 1 int("3.4")
ValueError: invalid literal for int() with base 10: '3.4'
```

However, Python 3 throws an error. Why? To be consistent, possibly. If you ask Python to perform two consecutive
typecasts, you must convert it explicitly in code.
```python
int(float("3.6"))
```

```output
3
```
:::
:::

::: challenge
 Arithmetic with Different Types

Which of the following will return the floating point number `2.0`?
Note: there may be more than one right answer.

```python
first = 1.0
second = "1"
third = "1.1"
```

1. `first + float(second)`
2. `float(second) + float(third)`
3. `first + int(third)`
4. `first + int(float(third))`
5. `int(first) + int(float(third))`
6. `2.0 * second`

::: solution
Answer: 1 and 4
:::
:::

## Use an index to get a single character from a string.

![Python uses 0-based indexing.](fig/03_0-based_indexing.png)

*   The characters (individual letters, numbers, and so on) in a string are
    ordered. For example, the string `'AB'` is not the same as `'BA'`. Because of
    this ordering, we can treat the string as a list of characters.
*   Each position in the string (first, second, etc.) is given a number. This
    number is called an **index** or sometimes a subscript.
*   Indices are numbered from 0.
*   Use the position's index in square brackets to get the character at that
    position.
    
```python
atom_name = 'sodium'
print(atom_name[0])
```

```output
s
```

## Use a slice to get a substring.

*   A part of a string is called a **substring**. A substring can be as short as a
    single character.
*   An item in a list is called an element. Whenever we treat a string as if it
    were a list, the string's elements are its individual characters.
*   Slicing gets a part of a string (or, more generally, a part of any list-like thing).
*   Slicing uses the notation `[start:stop]`, where `start` is the integer
    index of the first element we want and `stop` is the integer index of
    the element _just after_ the last element we want.
*   The difference between `stop` and `start` is the slice's length.
*   Taking a slice does not change the contents of the original string. Instead,
    taking a slice returns a copy of part of the original string.

```python
atom_name = 'sodium'
print(atom_name[0:3])
```

```output
sod
```

## Use the built-in function `len` to find the length of a string.

```python
print(len('sodium'))
```

```output
6
```

*   Nested functions are evaluated from the inside out,
     like in mathematics.


::: challenge
## Indexing
If you assign `a = 123`,
what happens if you try to get the second digit of `a` via `a[1]`?

::: solution
Numbers are not strings or sequences and Python will raise an error if you try to perform an index operation on a
number. In the [next lesson on types and type conversion]({{ page.root }}/03-types-conversion/#convert-numbers-and-strings)
we will learn more about types and how to convert between different types. If you want the Nth digit of a number you
 can convert it into a string using the `str` built-in function and then perform an index operation on that string.

```python
a = 123
print(a[1])
```

```output
TypeError: 'int' object is not subscriptable
```
```python
a = str(123)
print(a[1])
```

```output
2
```
:::
:::

::: challenge
## Slicing practice
What does the following program print?
```python
atom_name = 'carbon'
print('atom_name[1:3] is:', atom_name[1:3])
```

::: solution
```output
atom_name[1:3] is: ar
```
:::
:::

::: challenge
## Slicing concepts

Given the following string:
 
```python
species_name = "Acacia buxifolia"
```

What would these expressions return?

1.  `species_name[2:8]`
2.  `species_name[11:]` (without a value after the colon)
3.  `species_name[:4]` (without a value before the colon)
4.  `species_name[:]` (just a colon)
5.  `species_name[11:-3]`
6.  `species_name[-5:-3]`
7.  What happens when you choose a `stop` value which is out of range? (i.e., try `species_name[0:20]` or `species_name[:103]`)

::: solution

1.  `species_name[2:8]` returns the substring `'acia b'`
2.  `species_name[11:]` returns the substring `'folia'`, from position 11 until the end
3.  `species_name[:4]` returns the substring `'Acac'`, from the start up to but not including position 4
4.  `species_name[:]` returns the entire string `'Acacia buxifolia'`
5.  `species_name[11:-3]` returns the substring `'fo'`, from the 11th position to the third last position
6.  `species_name[-5:-3]` also returns the substring `'fo'`, from the fifth last position to the third last
7.  If a part of the slice is out of range, the operation does not fail. `species_name[0:20]` gives the same result as `species_name[0:]`, and `species_name[:103]` gives the same result as `species_name[:]`
:::
:::
