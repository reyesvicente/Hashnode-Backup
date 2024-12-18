---
title: "Python If Else and Code Branching"
datePublished: Mon Apr 22 2019 16:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cks3dlzs30vk3ees1hlj3eght
slug: python-if-else-and-code-branching
canonical: https://highcenburg.tech.blog/2019/04/23/python-if-else-and-code-branching/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771240052/0d746dad-991f-42e4-a97e-730dc380b7d7.jpeg
tags: python

---

Python, like most programming languages, has an if statement that provides branching in your code. An example syntax of Python’s if statement:

```python
x = 3
y = 4

if x == y:
    print("They are equal")
else:
    print("They are not equal")
```

The else branch is optional:

```python
if x == y:
    print("They are equal")
```

The expression can be anything that evaluates a True or False Example:

1. if num &gt;= 5:
    
2. if str == “What’s up?”:
    
3. if this != that:
    
4. if SomeVar:
    

Take note of example 4 above — in Python, anything that does not equate to zero, Null, or an empty object is True. Example:

```python
>>> s = 0
>>> if s:
...     print("True")
...    # Python returns nothing - statement is false
>>> s = 1
>>> if s:
...     print("True")
...
>>> s = " "
>>> if s:
...     print("True")
...     # Nothing again- statement is false
>>> s = "Hello"
>>> if s:
...     print("True")
...     
True
```

Python includes comprehensive range of boolean operators you can use within your expressions:

&lt; is Less than &lt;= is Less than equal &gt; is Greater than &gt;= is Greater than or equal == is Equal != is Not equal is Is a particular object is not Is not a particular object

Boolean operations are also supported for negating and chaining expressions:

```python
    or is Either expression can be True
    and is Both expressions must be True
    not is Negate the preceeding expression
```

Python also supports multiple branching using the elif statement:

```python
if [exp1 is True]:
   # execute if exp1 is True
elif [exp2 is True]:
   # execute if exp2 is True
elif [exp3 is True]:
   # execute if exp2 is True
```

Reference:

Build your first website with Python and Django: Build and Deploy a website with Python & Django