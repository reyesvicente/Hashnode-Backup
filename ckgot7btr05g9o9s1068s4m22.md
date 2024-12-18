---
title: "Django Startup Sequence"
datePublished: Mon Apr 22 2019 15:47:12 GMT+0000 (Coordinated Universal Time)
cuid: ckgot7btr05g9o9s1068s4m22
slug: django-startup-sequence
canonical: https://highcenburg.tech.blog
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771407653/8b125b3e-671f-4f59-b6f2-8533c66b2cb8.jpeg
tags: django, python3

---

*Originally posted on https://highceburg.tech.blog*

# Initializing project directory, installing the virtual environment and django:

```python
mkdir _project_name_
cd project_name
virtualenv _dir_name_
. ./_dir_name_/bin/activate
pip3 install django
```

To test if the installation worked, inside the virtual environment command prompt, start the interactive interpreter by typing

```python
python3
```

If the installation is successful, you should be able to import the django module and check the what version was installed:

```python
Python 3.7.3 (default, Mar 27 2019, 09:23:15)
[Clang 10.0.1 (clang-1001.0.46.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```

```python
>>> import django
>>> django.get_version()
'2.2'
>>>_
```

Exit interpreter once done.

# Starting a project:

```python
django-admin startproject _app_name_ 
```

# Creating a Database:

Inside the created folder in the last step, run the following command:

```python
python3 manage.py migrate
```

This creates a sqlite database and any necessary database tables. If all goes well, this is what you'll see:

```python
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
```

# The Development Server:

To verify the migration, run:

```python
python3 manage.py runserver
```

You will see the following output on the command line:

```python
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
April 22, 2019 - 15:24:21
Django version 2.2, using settings 'app.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

That's all folks!

###### Reference:

###### [Build your first website with Python and Django: Build and Deploy a website with Python & Django](https://amzn.to/2IypHGB)

[![](//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=US&ASIN=0994616864&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=vagr888-20 align="left")](https://www.amazon.com/gp/product/0994616864/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0994616864&linkCode=as2&tag=vagr888-20&linkId=fcc76125667c318f3ab0f253eb86ff5d)

![](//ir-na.amazon-adsystem.com/e/ir?t=vagr888-20&l=am2&o=1&a=0994616864 align="left")