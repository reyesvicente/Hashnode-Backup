---
title: "Django 2.2 Test Driven Development"
datePublished: Tue May 07 2019 23:20:35 GMT+0000 (Coordinated Universal Time)
cuid: ckgot7sgj05gao9s1g961dwob
slug: django-22-test-driven-development
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771057440/91fe905a-9e4d-4b00-9260-70ed5a900182.jpeg
tags: unit-testing, django, python3

---

95% into my first Django Website built by yours truly with the help of this [book](https://www.amazon.com/gp/product/0994616864/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0994616864&linkCode=as2&tag=vagr888-20&linkId=fcc76125667c318f3ab0f253eb86ff5d) and something came into my mind. Testing.

I recently stumbled upon the words of [Jacob Kaplan-Moss](https://jacobian.org/), one of Django's original creators, "Code without tests is broken as designed." this hit me in a way that I know to my self that I suck at designing, which motivated me to learn how to test.

### "Writing tests is important because it automates the process of confirming that the code works as expected." - [Django For Beginners](https://djangoforbeginners.com/pages-app/)

```python
AssertionError: 301 != 200
```

[SimpleTestCase](https://docs.djangoproject.com/en/2.2/topics/testing/tools/#django.test.SimpleTestCase)[unittest.TestCase](https://docs.python.org/3/library/unittest.html#unittest.TestCase)

```python
from django.test import SimpleTestCase

class PageTest(SimpleTestCase):
    def test_portfolio_page_status_code(self):
        response = self.client.get("/portfolio")
        self.assertEquals(response.status_code, 200)
    def test_blog_page_status_code(self):
        response = self.client.get("/blog")
        self.assertEqual(response.status_code, 200)
    def test_home_page_status_code(self):
        response = self.client.get("")
        self.assertEquals(response.status_code, 200)
    def test_quotes_page_status_code(self):
        response = self.client.get("/quotes/")
        self.assertEquals(response.status_code, 200)
```

```python
======================================================================
FAIL: test_blog_page_status_code (personal_portfolio.tests.PageTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/user/dir/personal_portfolio/tests.py", line 
11, in test_blog_page_status_code
self.assertEqual(response.status_code, 200)
AssertionError: 301 != 200

======================================================================
FAIL: test_portfolio_page_status_code 
(personal_portfolio.tests.PageTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/user/dir/personal_portfolio/tests.py", line 
7, in test_portfolio_page_status_code
self.assertEquals(response.status_code, 200)
AssertionError: 301 != 200
```

I went to StackOverflow to ask for help and this is what I got:

[It is likely that your site does in fact return a `301` redirect code. This can happen for many reasons, but for example, if it requires authentication and redirects the some other page for that purpose. If so, one fix can be to add a test that logs in and then goes to that page. Without more information about how your site works (and the code) it's hard to determine the cause.](https://stackoverflow.com/a/56030700/11292732)

[TransactionTestCase](https://docs.djangoproject.com/en/2.2/topics/testing/tools/#django.test.TransactionTestCase)[SimpleTestCase](https://docs.djangoproject.com/en/2.2/topics/testing/tools/#django.test.SimpleTestCase)

```python
from django.test import TransactionTestCase


class Personal_PortfolioTests(TransactionTestCase):
    def test_home_page_status_code(self):
        response = self.client.get('/')
        self.assertEqual(response.status_code,200)
    def test_portfolio_page_status_code(self):
        response = self.client.get('/portfolio/')
        self.assertEqual(response.status_code,200)
    def test_quotes_page_status_code(self):
        response = self.client.get('/quotes/')
        self.assertEqual(response.status_code,200)
    def test_blog_page_status_code(self):
        response = self.client.get('/blog/')
        self.assertEqual(response.status_code,200)
```

## And viola!

![test](https://highcenburgtech.files.wordpress.com/2019/05/test.png align="left")

Django’s [`TestCase`](https://docs.djangoproject.com/en/2.2/topics/testing/tools/#django.test.TestCase) class is a more commonly used subclass of `TransactionTestCase` that makes use of database transaction facilities to speed up the process of resetting the database to a known state at the beginning of each test. A consequence of this, however, is that some database behaviors cannot be tested within a Django `TestCase` class. For instance, you cannot test that a block of code is executing within a transaction, as is required when using [`select_for_update()`](https://docs.djangoproject.com/en/2.2/ref/models/querysets/#django.db.models.query.QuerySet.select_for_update). In those cases, you should use `TransactionTestCase`.

Read more here: https://docs.djangoproject.com/en/2.2/topics/testing/tools/#django.test.TransactionTestCase