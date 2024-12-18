---
title: "Classes and Functions"
datePublished: Mon Apr 22 2019 16:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cks3djset0vfyhqs1b14q84n0
slug: classes-and-functions
canonical: https://highcenburg.tech.blog/2019/04/23/classes-and-functions/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771287728/6e892d77-69f3-4130-89fc-289f1ce8e4ae.jpeg
tags: python, django

---

The basic purpose of classes and functions is to group together pieces of related code. The major difference between the two is that a function does something whereas a class is something.

For example, if person was a class, `walk()` and `eat()` would be functions.

Both classes and functions can contain other functions. If a function is inside another function, it’s called a sub-function. If a function is included inside a class, it’s called a method. Subclasses also exist, but they are created by a process called inheritance.

Let’s define a function using the def statement:

```python
def function_name([parameter list]):
    # rest of function
```

To define a class in python, you use the class statement:

```python
class Someclass([argument list]):
    # class constructor
    __init__():
        # Constructor code
    # class methods
    def ...
```

You create subclasses that contain all the attributes and methods of another class using inheritance. Inheritance is an important concept in object-oriented programming. Inheritance helps prevent repetition in code, and it allows programmers to build complex programs from simpler building blocks.

To create a class that inherits from another class, you refer to the parent when defining the class:

```python
class ChildClass(ParentClass):
```

This is easier to understand with an example–check out this form class:

```python
class ContactForm(forms.Form):
    subject = forms.CharField(max_length=100)
    email = forms.EmailField(required=False)
    message = forms.CharField(widget=forms.Textarea)
```

In this example, the class inherits from Django’s forms.Form class, which make all of the methods and attributes of the parent class (forms.Form) available in the child class (subclass) ContactForm.