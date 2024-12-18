---
title: "Designing Better Models in Django"
datePublished: Sat Mar 06 2021 13:08:40 GMT+0000 (Coordinated Universal Time)
cuid: cklxqt1xk0208fss16q8r0zfe
slug: designing-better-models-in-django
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1615036041686/OlE2m81xB.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1615036110573/qJjFn50lJ.jpeg
tags: python, django

---

*Photo by* [*Christopher Gower*](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *on* [*Unsplash*](https://unsplash.com/s/photos/django-web-framework?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## Introduction

%[https://www.youtube.com/watch?v=lTRiuFIWV54] 

Defining database models is the most critical part of a new project. Django’s [coding style](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#model-style) ives us a recommended way to create models for our apps which I only discovered today thanks to [LearnDjango](https://learndjango.com).:grin: Creating database models may not be simple to devs who haven’t laid hands on Django yet, but I’ll try my best to make sense. :grin:

[Learn Django](https://learndjango.com) is one of my go-to resources in being a Django developer. If you haven’t checked out the article about my top Django resources, you can check it [here](https://www.icenreyes.xyz/posts/my-top-5-django-resources/).

## Model and Field Names

Models will always start with a Capital letter and will always be singular because they should only represent a single object and be using underscores and not a camelCase.

```bash
from django.db import models

class City(models.Model):
	town_name = models.CharField(max_length=50)
```

## Common Methods

Defining models is not easy nor should not difficult. Next we add the `__str__` and `get_absolute_url()` methods.

Defining the `str` method makes the model human-readable in the Django admin.

Defining the `get_absolute_url()` method tells Django how to calculate the canonical URL for the model. This method should return a string that references a **view** on the site. It’s also a good practice to use `get_absolute_url()` on the templates instead of hard-coding them.

`<a href="town/{{ object.id }]}/">{{ object.town_name }}</a>` # wrong

`<a href="{{ object.get_absolute_url }}/">{{ object.town_name }}</a>` # better

```bash
from django.db import models
from django.urls import reverse # new

class City(models.Model):
	town_name = models.CharField(max_length=50)
	
	def __str__(self): # new
		return self.town_name
	
	def get_absolute_url(self):
		return reverse('town', kwargs={"town_name": self.town_name})
```

## Explicit Naming

#### [Explicit is better than implicit](https://www.python.org/dev/peps/pep-0020)

There are two ways to implicitly set the human-readable name for each field which Django does by automatically converting underscores to spaces. One is to use verbose\_name(); if the field type is not **OneToOneField**, **ManyToMany** or **ForeignKey**, you can set it as the first positional argument.

```bash
from django.db import models 

class City(models.Model): 
	town_name = models.CharField(
	max_length=50,
	verbose_name='town name',
	)

	def __str__(self):
		return self.town_name 
	
	def get_absolute_url(self): 
		return reverse('town', kwargs={"town_name": self.town_name})
```

If a model uses **ForeignKey**, the model will have access to a Manager that returns all instances of the first model. This model returns **QuerySets** which can be filtered to retrieve objects.

*Example:*

```bash
>>> a = City.objects.get()
```

```bash
...
class Mayor(models.Model):
	first_name = models.CharField('first name', max_length=30)
	last_name = models.CharField('last name', max_length=30)
	city = models.ForeignKey(
		City,
		on_delete=models.CASCADE,
		related_name='mayors',
		related_query_name='human',
		)
		
	def __str__(self):
		return '%s %s' % (self.first_name, self.last_name)
	
	def get_absolute_url(self):
		return reverse('town_mayor', kwargs={'town_name': self.town_name})
```

### References

* [Coding style | Django documentation | Django](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#model-style)
    
* [Django Best Practices: Models | LearnDjango](https://learndjango.com/tutorials/django-best-practices-models)
    
* [Model instance reference | Django documentation | Django](https://docs.djangoproject.com/en/3.0/ref/models/instances/#get-absolute-url)
    
* [PEP 20 -- The Zen of Python | Python.org](https://www.python.org/dev/peps/pep-0020/)
    
* [Making queries | Django documentation | Django](https://docs.djangoproject.com/en/3.0/topics/db/queries/#backwards-related-objects)
    

```bash
In [1]: from blog.source.models import Category 
In [2]: a = Category.objects.get(name='python') 
In [3]: print(a) python
```