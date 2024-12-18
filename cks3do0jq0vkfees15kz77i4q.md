---
title: "List, Dictionary & Tuples"
datePublished: Mon Apr 22 2019 16:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cks3do0jq0vkfees15kz77i4q
slug: list-dictionary-and-tuples
canonical: https://highcenburg.tech.blog/2019/04/23/list-dictionary-tuples/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771199006/90e3dd5b-b291-449b-9275-323c4dd4384a.jpeg
tags: python

---

Lists, dictonaries & tuples are used to store collections of objects. They are differentiated from each other by the delimiter they use:

```python
        A list : [ ]

        ["one", "two"]

        A dictionary : { }

        {1: "one", 2: "two"}

        A tuple : ( )

        ("one", 2)
```

You should be very familiar with Lists as we all use this everyday. A dictionary is pretty straight forward, think of a regular dictionary where you have the word and then the meaning of the word. In Python, the word is called a key and the definition a value.

Tuples are like lists, with a couple of differences. Lists are designed to contain largely homogeneous data, much like in real life where you would have a shopping list or a to do list. By convention, Python list items should all be the same type.

On the other hand, Tuples are used to store heterogeneous data —

`("one", 2, [three])`

is perfectly acceptable as a tuple where it would be frowned upon as a list. A single element tuple(singleton) is also written differently to a single element list:

```python
lst = ["one"] # 1 element list
tpl = ("one",) # 1 element tuple with trailing comma to differentiate between a plain string ("one") or a functions parameter some_func("one")
```

Tuples, like strings, are immutable. Once they are created, you can not change them. However, Lists and Dictionaries, are mutable and changable.

Example:

```python
>>> lst = ["one"] # List, Tuple & Dictionary setup
>>> tpl = ("one",)
>>> dict = {0: "one"}
>>> lst[0] # All contain the same first element 'one'
'one'
>>> tpl[0]
'one'
>>> dict[0]
'one'
>>> lst[0] = "two" # List is mutable ( can be changed )
>>> lst
['two']
>>> dict[0] = "two" # So is the dictionary
>>> dict
{0: 'two'}
>>> tpl[0] = "two" # Tuple is immutable ( can not be changed )
>>> Traceback (most recent call last):
>>>  File "", line 1, in 
>>> TypeError: 'tuple' object does not support item assignment
>>> str = "one" # String is also immutable
>>> str[0] = "x"
>>> Traceback (most recent call last):
>>>  File "", line 1, in 
>>> TypeError: 'str' object does not support item assignment
```

Tuples are often used for homogenous data that the programmer doesn’t want changed by accident. It’s a good idea to use a tuple instead of a list if you have a list of items that should not be changed.

Reference:

Build your first website with Python and Django: Build and Deploy a website with Python & Django