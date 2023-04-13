---
title: "String Manipulation"
teaching: 10
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions 

- "How can I make a program do many things?"
- "How can I do something for each thing in a list?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Explain what for loops are normally used for."
- "Trace the execution of a simple (unnested) loop and correctly state the values of variables in each iteration."
- "Write for loops that use the Accumulator pattern to aggregate values."

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- "A *for loop* executes commands once for each value in a collection."
- "A `for` loop is made up of a collection, a loop variable, and a body."
- "The first line of the `for` loop must end with a colon, and the body must be indented."
- "Indentation is always meaningful in Python."
- "Loop variables can be called anything (but it is strongly advised to have a meaningful name to the looping variable)."
- "The body of a loop can contain many statements."
- "Use `range` to iterate over a sequence of numbers."

::::::::::::::::::::::::::::::::::::::::::::::::

## Strings can be indexed and sliced.

As we saw in class during the [types](03-types.md) lesson, strings in Python can be **indexed** and **sliced**

```python
my_string = "What a lovely day."

# Indexing
my_string[2]

# Slicing
my_string[1:3]
my_string[:3] # Same as my_string[0:3]
my_string[1:] # Same as my_string[1:len(my_string)]
```

## Strings are immutable.

We will talk about this concept in more detail when we explore [lists](06-lists.md)

## Build complex strings based on other variables using format.

## Python has many useful built-in functions for string manipulation.

Python has many builtin methods for manipulating strings; simple and powerful text manipulation is considered one of Python's strengths.
We will go over some of the more common and useful functions here, but be aware that there are many more you can find in the [official documentation](https://docs.python.org/3/library/string.html).

### Dealing with whitespace

`str.strip()`

`str.split()`

### Pattern matching

`str.find()` and `str.rfind()`

`str.startswith()` and `str.endswith()`

### Case

`str.upper()`, `str.lower()`, `str.capitalize()`, and `str.title()`