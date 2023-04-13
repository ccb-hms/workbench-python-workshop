---
title: "Using Objects"
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- "What is an object?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Define objects."
- "Use an object's methods."
- "Call a constructor for an object."

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- "Objects are entities with both **data** and **methods**"
- "Methods are unique to objects, and so methods with the same name may work differently on different objects."
- "You can create an object using a **constructor**."
- "Objects need to be explicitly copied."

::::::::::::::::::::::::::::::::::::::::::::::::

## Objects are entities with both **data** and **methods**

In addition to basic types, Python also has objects we refer to the type of an object as its *class*.
In other programming languages there is a more definite distinction between base types and objects, but in Python these terms essentially interchangable. 
However, thinking about more complex data structures through an *object-oriented* lense will allow us to better understand how to write effective code. 

We can think of an object as an entity which has two aspects: data and methods.
The data (sometimes called properties) is what we would typically think of as the value of that object; what it is storing. 
Methods define what we can do with an object, essentially they are functions specific to an object (you will often hear function and method used interchangebly). 

Next session we will explore the `list` object in Python. 
A list consists of its data, or what is stored in the list. 

```python
# Here, we are storing 3, 67, 1, and 33 as the data inside the list object
sizes = [3,67,1,33]

# Reverse is a list method which reverses that list
sizes.reverse()

print(sizes)
```

```output
[33, 1, 67, 3]
```

## Methods are unique to objects, and so methods with the same name may work differently on different objects.

In Python, many objects share method names. 
However, those methods may do different things.

We have already seen this implicitly looking at how operations like addition interact with different basic types. 
For instance, performing multiplication on a list:

```python
sizes * 3
```
```output
[33, 1, 67, 3, 33, 1, 67, 3, 33, 1, 67, 3]
```
May not have the behavior you expect. 
Whenever our code isn't doing what we want, one of the first things to check is that the `type` of our variables is what we expect. 

## You can create an object using a **constructor**.

All objects have constructors, which are special function that create that object. 
Constructors are often called implicitly when we, for instance, define a list using the `[]` notation or create a Pandas `dataframe` object using `read_csv`.
However, objects also have explicit constructors which can be called directly.

```python
# The basic list constructor
new_sizes = list()

new_sizes.append(33)
new_sizes.append(1)
new_sizes.append(67)
new_sizes.append(3)

print(new_sizes)
```

```output
[33, 1, 67, 3]
```

## Objects need to be explicitly copied.

We will continue to circle back to this point, but one imporant note about complex objects is that they need to be explicitly copied. 
This is due to them being **mutable**, which we will discuss more next session. 

We can have multiple variables refer to the same object. 
This can result in us unknowningly changing an object we didn't expect to. 

For instance, two variables can refer to the same list:

```python
# Here, we lose the original sizes list and now both variables are pointing to the same list
sizes = new_sizes
print(sizes)
print(new_sizes)

# Thus, if we change sizes:
sizes.append(100)

# We see that new_sizes has also changed
print(sizes)
print(new_sizes)
```

```output
[33, 1, 67, 3]
[33, 1, 67, 3]
[33, 1, 67, 3, 100]
[33, 1, 67, 3, 100]
```

In order to make a variable refer to a different copy of a list, we need to explicitly copy it. 
One way to do this is to use a *copy constructor*. 
Most objects in Python have a copy constructor which accepts another object of the same type and creates a copy of it. 

```python
# Calling the copy constructor
sizes = list(new_sizes)
print(sizes)
print(new_sizes)

# Now if we change sizes:
sizes.append(100)

# We see that new_sizes does NOT change
print(sizes)
print(new_sizes)
```

```output
[33, 1, 67, 3, 100]
[33, 1, 67, 3, 100]
[33, 1, 67, 3, 100, 100]
[33, 1, 67, 3, 100]
```

However, even copying an object can sometimes not be enough. 
Some objects are able to store other objects. 
If this is the case, the internal object might not change. 

```python
# Multiplying lists gets weird
list_of_lists = [sizes] * 4
print(list_of_lists)
```

```output
[[33, 1, 67, 3, 100, 100], [33, 1, 67, 3, 100, 100], [33, 1, 67, 3, 100, 100], [33, 1, 67, 3, 100, 100]]
```

```python
# While the external list here is different, the internal lists are still the same
new_lol = list(list_of_lists)
print(new_lol)
```

```output
[[33, 1, 67, 3, 100, 100], [33, 1, 67, 3, 100, 100], [33, 1, 67, 3, 100, 100], [33, 1, 67, 3, 100, 100]]
```

```python
new_lol[0].reverse()
print(list_of_lists)
```

```output
[[100, 3, 67, 1, 33], [100, 3, 67, 1, 33], [100, 3, 67, 1, 33], [100, 3, 67, 1, 33]]
```

Overall, we need to be careful in Python when copying objects.
This is one of the reasons why most libaries' methods return new objects as opposed to altering existing objects, so as to avoid any confusion over different object versions. 
Additionally, we can perform a *deep copy* of an object, which is an object copy which copys and stored objects. 

::: challenge

Pandas is one of the most popular python libraries for manipulating data which we will be diving into next week.
Take a look at the official documentation for Pandas and try to find how to create a deep copy of a dataframe. 
Is what you found a function or a method?
You can find the documentation [here](https://pandas.pydata.org/docs/index.html)

:::