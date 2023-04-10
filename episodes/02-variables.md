---
title: "Variables in Python"
teaching: 20
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do I run Python code?
- What are variables?
- How do I set variables in Python?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use the Python console to perform calculations.
- Assign values to variables in Python. 
- Reuse variables in Python. 

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Python is an interpreted programming language, and can be used interactively. 
- **Values** are assigned to **variables** in Python using `=`.
- You can use `print` to output variable values. 
- Use meaningful variable names.


::::::::::::::::::::::::::::::::::::::::::::::::

## Variables store values.

*   **Variables** are names for values.
*   In Python the `=` symbol assigns the value on the right to the name on the left.
*   The variable is created when a value is assigned to it.
*   Here, Python assigns an age to a variable `age`
    and a name in quotes to a variable `first_name`.

```python
age = 42
first_name = 'Ahmed'
```

*   Variable names
    * can **only** contain letters, digits, and underscore `_` (typically used to separate words in long variable names)
    * cannot start with a digit
    * are **case sensitive** (age, Age and AGE are three different variables)
*   Variable names that start with underscores like `__alistairs_real_age` have a special meaning
    so we won't do that until we understand the convention.

## Use `print` to display values.

*   Python has a built-in function called `print` that prints things as text.
*   Call the function (i.e., tell Python to run it) by using its name.
*   Provide values to the function (i.e., the things to print) in parentheses.
*   To add a string to the printout, wrap the string in single or double quotes.
*   The values passed to the function are called **arguments**

```python
print(first_name, 'is', age, 'years old')
```
```output
Ahmed is 42 years old
```

*   `print` automatically puts a single space between items to separate them.

## Variables must be created before they are used.

*   If a variable doesn't exist yet, or if the name has been mis-spelled,
    Python reports an error. (Unlike some languages, which "guess" a default value.)

```python
print(last_name)
```
```output
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-1-c1fbb4e96102> in <module>()
----> 1 print(last_name)

NameError: name 'last_name' is not defined
```

*   The last line of an error message is usually the most informative.
*   We will look at error messages in detail later.

::: callout
## Variables Persist Between Cells
Be aware that it is the *order* of execution of cells that is important in a Jupyter notebook, not the order
in which they appear. Python will remember *all* the code that was run previously, including any variables you have
defined, irrespective of the order in the notebook. Therefore if you define variables lower down the notebook and then
(re)run cells further up, those defined further down will still be present. As an example, create two cells with the
following content, in this order:

```python
print(myval)
```

```python
myval = 1
```
If you execute this in order, the first cell will give an error. However, if you run the first cell *after* the second
cell it will print out `1`. To prevent confusion, it can be helpful to use the `Kernel` -> `Restart & Run All` option which clears the interpreter and runs everything from a clean slate going top to bottom.
:::

## Variables can be used in calculations.

*   We can use variables in calculations just as if they were values.
    *   Remember, we assigned the value `42` to `age` a few lines ago.

```python
age = age + 3
print('Age in three years:', age)
```
```output
Age in three years: 45
```
## Python is case-sensitive.

*   Python thinks that upper- and lower-case letters are different,
    so `Name` and `name` are different variables.
*   There are conventions for using upper-case letters at the start of variable names so we will use lower-case letters for now.

## Use meaningful variable names.

*   Python doesn't care what you call variables as long as they obey the rules
    (alphanumeric characters and the underscore).

```python
flabadab = 42
ewr_422_yY = 'Ahmed'
print(ewr_422_yY, 'is', flabadab, 'years old')
```

*   Use meaningful variable names to help other people understand what the program does.
*   The most important "other person" is your future self.

::: challenge
## Swapping Values

Fill the table showing the values of the variables in this program
*after* each statement is executed.

```python
# Command  # Value of x   # Value of y   # Value of swap #
x = 1.0    #              #              #               #
y = 3.0    #              #              #               #
swap = x   #              #              #               #
x = y      #              #              #               #
y = swap   #              #              #               #
```
::: hint

Try using this [python visualization tool](https://pythontutor.com/visualize.html#code=x%20%3D%201.0%20%20%20%0Ay%20%3D%203.0%20%0Aswap%20%3D%20x%20%0Ax%20%3D%20y%20%20%20%20%20%20%0Ay%20%3D%20swap%20&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false) to visualize what happens in the code. 

:::

::: solution
```output
# Command  # Value of x   # Value of y   # Value of swap #
x = 1.0    # 1.0          # not defined  # not defined   #
y = 3.0    # 1.0          # 3.0          # not defined   #
swap = x   # 1.0          # 3.0          # 1.0           #
x = y      # 3.0          # 3.0          # 1.0           #
y = swap   # 3.0          # 1.0          # 1.0           #
```

These three lines exchange the values in `x` and `y` using the `swap`
variable for temporary storage. This is a fairly common programming idiom.
:::
:::

::: challenge
## Predicting Values

What is the final value of `position` in the program below?
(Try to predict the value without running the program,
then check your prediction.)

```python
initial = 'left'
position = initial
initial = 'right'
```

::: solution
```python
print(position)
```

```output
left
```

The `initial` variable is assigned the value `'left'`.
In the second line, the `position` variable also receives
the string value `'left'`. In third line, the `initial` variable is given the
value `'right'`, but the `position` variable retains its string value
of `'left'`.
:::
:::

::: challenge
## Choosing a Name

Which is a better variable name, `m`, `min`, or `minutes`?
Why?

::: hint
Hint: think about which code you would rather inherit from someone who is leaving the lab:

1. `ts = m * 60 + s`
2. `tot_sec = min * 60 + sec`
3. `total_seconds = minutes * 60 + seconds`
:::
::: solution
`minutes` is better because `min` might mean something like "minimum"
(and actually is an existing built-in function in Python that we will cover later).
:::
:::

