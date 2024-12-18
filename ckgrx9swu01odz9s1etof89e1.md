---
title: "Testing our app using Unittests"
datePublished: Tue Oct 27 2020 08:41:26 GMT+0000 (Coordinated Universal Time)
cuid: ckgrx9swu01odz9s1etof89e1
slug: testing-our-app-using-unittests
canonical: https://learnetto.com/tutorials/part-vi-testing-our-app-using-unittests
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682767594464/bb05b43b-3dd8-4cc9-89e2-5831578a47bc.jpeg
tags: tutorial, django, python3, beginners

---

Suppose you've gotten familiar with our project directory. In that case, you should've noticed the testing, code quality packages, and linters installed earlier when we installed the development requirements. In testing our models, views & forms, we'll stick with the most common Django test class in writing our tests.

source: https://docs.djangoproject.com/en/3.0/topics/testing/tools/#testcase

In your main app, create a test directory and inside the directory, create a test_models.py file. 

``` python
# blog_tutorial/main/tests/test_models.py
from django.test import TestCase

from blog_tutorial.main.models import Contact


class ContactModelTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        # Setup data on form
        Contact.objects.create(name="Elon Musk", email="elon@musk.com", message="This is a message")

    def test_form_content(self):
        contact = Contact.objects.get(id=1)
        expected_objects_in_form = f'{contact.name}', f'{contact.email}', f'{contact.message}'
        self.assertEquals(expected_objects_in_form, ('Elon Musk', 'elon@musk.com', 'This is a message'))

    def test_name_label(self):
        contact = Contact.objects.get(id=1)
        field_label = contact._meta.get_field('name').verbose_name
        self.assertEquals(field_label, 'name')

    def test_email_label(self):
        contact = Contact.objects.get(id=1)
        field_label = contact._meta.get_field('email').verbose_name
        self.assertEquals(field_label, 'email')

    def test_message_label(self):
        contact = Contact.objects.get(id=1)
        field_label = contact._meta.get_field('message').verbose_name
        self.assertEquals(field_label, 'message')

    def test_name_max_length(self):
        contact = Contact.objects.get(id=1)
        max_length = contact._meta.get_field('name').max_length
        self.assertEquals(max_length, 100)

    def test_email_max_length(self):
        contact = Contact.objects.get(id=1)
        max_length = contact._meta.get_field('email').max_length
        self.assertEquals(max_length, 50)
```

We're testing our fields' value in our Contact model and the max_length parameter to check if the values are as expected. 

``` bash
$ python manage.py test blog_tutorial.main.tests.test_models
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.....
----------------------------------------------------------------------
Ran 6 tests in 0.015s # initial tests ran

OK
Destroying test database for alias 'default'...
```

One down, X to go. X simply means we'll try to pass all tests before we move on to the next chapter.

``` python
# blog_tutorial/main/tests/test_models.py

class ProjectModelTest(TestCase):
    
    def setUp(self):
        Project.objects.create(
            title="Acme",
            slug="acme",
            live_site="www.example.com",
            github_link="www.github.com/learnetto/eventlite",
            description="Example description",
        )

    def test_project_data(self):
        project_details = Project.objects.get(id=1)
        expected_object_details = f'{project_details.title}', f'{project_details.slug}', f'{project_details.live_site}', f'{project_details.github_link}', f'{project_details.description}'
        self.assertEqual(expected_object_details, ("Acme", "acme", "www.example.com", "www.github.com/learnetto/eventlite", "Example description"))

    def project_list_view(self):
        response = self.client.get(reverse('project_details'))
        self.assertEqual(response.status_code, 200)
        self.assertContacts(response, ("Acme", "www.example.com", "www.github.com/learnetto/eventlite", "Example description"))
        self.assertTemplateUsed(response, 'pages/projects.html')

# shell
$ python manage.py test blog_tutorial.main.tests.test_models
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
......
----------------------------------------------------------------------
Ran 7 tests in 0.023s # added 1 test

OK
Destroying test database for alias 'default'...
```

Okay, that was the Project model test result. We're testing the Post model next, but the post's categories object will be tested with on the Category model.

``` python
# blog_tutorial/main/tests/test_models.py

class PostModelTest(TestCase):
    
    def setUp(self):
        Post.objects.create(
            title="Test title",
            slug="test-title",
            overview="This is the overview.",
            body="This is the body.",
            image="image.jpg",
            created_on="2020-07-28",
            updated_on="2020-07-29",
            status="published",
        )

    def test_post_data(self):
        post = Post.objects.get(id=1)
        expected_post_details = f'{post.title}', f'{post.slug}', f'{post.overview}', f'{post.body}', f'{post.image}', f'{post.created_on}', f'{post.updated_on}', f'{post.status}'
        self.assertEqual(expected_post_details, ("Test title", "test-title", "This is the overview.", "This is the body.", "image.jpg", "2020-07-28", "2020-07-29", "published"))

    def post_list_view(self):
        response = self.client.get(reverse('post'))
        self.assertEqual(response.status_code, 200)
        self.assertContacts(response, ("Test title", "test-title", "This is the overview.", "This is the body.", "image.jpg", "2020-07-28", "2020-07-29", "published"))
        self.assertTemplateUsed(response, 'pages/home.html')

# shell


Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.......
----------------------------------------------------------------------
Ran 8 tests in 0.033s # added 1 test

OK
Destroying test database for alias 'default'...
```

Phew. The last model to be tested in the Category model. We have to ensure that the test passes since our Post model has a relationship to our Category model.

``` python
# blog_tutorial/main/tests/test_models.py

class CategoryModelTest(TestCase):

    def setUp(self):
        Category.objects.create(
            title="blog", 
            slug="blog", 
        )

    def test_category_data(self):
        category = Category.objects.get(id=1)
        expected_category_details = f'{category.title}', f'{category.slug}'
        self.assertEqual(expected_category_details, ("blog", "blog", ))

    def category_list_view(self):
        response = self.client.get(reverse('category'))
        self.assertEqual(response.status_code, 200)
        self.assertContacts(response, ("blog", "blog", ))
        self.assertTemplateUsed(response, 'pages/category_list.html')

# shell
$ python manage.py test blog_tutorial.main.tests.test_models

Creating test database for alias 'default'...
System check identified no issues (0 silenced).
........
----------------------------------------------------------------------
Ran 9 tests in 0.067s

OK
Destroying test database for alias 'default'...
```

Ok, now let's test our Contact Form and then our Views. 

``` python
# blog_tutorial/main/tests/test_contact_form.py

class ContactFormTest(TestCase):

    def test_contact_form_field_labels(self):
        form = ContactForm()
        self.assertTrue(form.fields['name'].label == None or form.fields['name'].label == 'Your name')
        self.assertTrue(form.fields['email'].label == None or form.fields['email'].label == 'Your email')
        self.assertTrue(form.fields['message'].label == None or form.fields['message'].label == 'Your inquiry')

    def test_contact_form_submission(self):
        form_data = {'name': "Walter White", 'email': "walterwhite@breakingbad.com", "message": 'I want some gasoline.'}
        form = ContactForm(data=form_data)
        self.assertTrue(form.is_valid(), form.errors)

    def test_contact_form_failed_submission(self):
        form = ContactForm(data = {'name': "Walter Brown", 'email': "", "message": ''})
        self.assertFalse(form.is_valid())

# shell

$ python manage.py test blog_tutorial.main.tests.test_contact_form

Creating test database for alias 'default'...
System check identified no issues (0 silenced).
...
----------------------------------------------------------------------
Ran 3 tests in 0.006s

OK
Destroying test database for alias 'default'...
```

Let's now test the views. We'll start off with our static pages since our about page is just a static page. 

Create a test_views.py in your tests directory.

``` python
# blog_tutorial/main/tests/test_views.py

from django.test import TransactionTestCase
from django.urls import reverse

from blog_tutorial.main.views import AboutView


class AboutPageTest(TransactionTestCase):
    def test_about_page_status_code(self):
        response = self.client.get('/about/')
        self.assertEquals(response.status_code, 200)

    def test_view_url_by_name(self):
        response = self.client.get(reverse('about'))
        self.assertEquals(response.status_code, 200)

    def test_view_uses_correct_template(self):
        response = self.client.get(reverse('about'))
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'pages/about.html')

    def test_about_page_contains_correct_html(self):
        response = self.client.get('/about/')
        self.assertContains(response, '<h1>Hello from the about test!</h1>')

    def test_about_page_does_not_contain_incorrect_html(self):
        response = self.client.get('/')
        self.assertNotContains(
            response, 'John Cena is here.')

$ python manage.py test blog_tutorial.main.tests.test_views
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.....
----------------------------------------------------------------------
Ran 5 tests in 1.462s

OK
Destroying test database for alias 'default'...
```

Let's now create the tests to list blogs, projects, category lists, and both models' details.

``` python
# blog_tutorial/main/tests/test_views.py

class TestPostListView(TestCase):

    def setUp(self):
        blog_posts = 10

        for post_id in range(blog_posts):
            Post.objects.create(
                title=f'Test title',
                slug=f'test-title',
                overview=f'This is the overview.', 
                body=f'This is the body.', 
                image=f'image.jpg', 
                created_on=f'2020-07-28', 
                updated_on=f'2020-07-29',
                status=f'published',
                )
    
    def test_if_url_exist(self):
        response = self.client.get('')
        self.assertEquals(response.status_code, 200)
    
    def test_if_view_accessible_by_name(self):
        response = self.client.get(reverse('blog'))
        self.assertEquals(response.status_code, 200)

    def test_if_view_uses_right_template(self):
        response = self.client.get(reverse('blog'))
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'pages/home.html')

    def test_if_pagination_is_three(self):
        response = self.client.get(reverse('blog'))
        self.assertEquals(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertTrue(len(response.context['posts']) == 3)

    def test_list_all_blog_posts(self):
         # Get second page and confirm it has (exactly) remaining 3 items
        response = self.client.get(reverse('blog')+'?page=2')
        self.assertEquals(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertTrue(len(response.context['posts']) == 3)

class TestPostDetailView(TestCase):

    def setUp(self):
        Post.objects.create(
            title=f"Test title", 
            slug=f"test-title", 
            overview=f"This is the overview.", 
            body=f"This is the body.", 
            image=f"image.jpg", 
            created_on=f"2020-07-28", 
            updated_on=f"2020-07-29",
            status=f"published",
            )

    def test_if_url_exist(self):
        response = self.client.get('/blog/test-title/')
        self.assertEqual(response.status_code, 200)

    def test_if_view_uses_right_template(self):
        response = self.client.get('/blog/test-title/')
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, 'pages/post_detail.html')


class TestProjectDetailView(TestCase):

    def setUp(self):
        Project.objects.create(
            title='Learnetto',
            slug="learnetto",
            image="learnetto.jpg",
            live_site='www.vgreyes.com',
            github_link='www.github.com/learnetto',
            description='This is a test description for the project.',
            )

    def test_if_url_exist(self):
        response = self.client.get('/portfolio/learnetto/')
        self.assertEquals(response.status_code, 200)

    def test_if_view_uses_right_template(self):
        response = self.client.get('/portfolio/learnetto/')
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'pages/project_details.html')


class TestProjectListView(TestCase):

    def setUp(self):
        project_list = 10

        for project_id in range(project_list):
            Project.objects.create(
                title=f'Acme',
                slug=f'acme',
                image=f'project_image.jpg',
                live_site=f'www.learnetto.com',
                github_link=f'www.github.com/reyesvicente',
                description=f'This is a test description',
                )

    def test_if_url_exist(self):
        response = self.client.get('/portfolio/')
        self.assertEquals(response.status_code, 200)

    def test_if_view_accessible_by_name(self):
        response = self.client.get(reverse('portfolio'))
        self.assertEquals(response.status_code, 200)

    def test_if_view_uses_right_template(self):
        response = self.client.get(reverse('portfolio'))
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'pages/projects.html')

    def test_if_pagination_is_five(self):
        response = self.client.get(reverse('portfolio'))
        self.assertEquals(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertTrue(len(response.context['projects']) == 5)

    def test_list_all_projects(self):
        response = self.client.get(reverse('portfolio')+'?page=2')
        self.assertEquals(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertTrue(len(response.context['projects']) == 5)


class TestCategoryView(TestCase):
    
    def setUp(self):
        Category.objects.create(
            title="Test Blog Category", 
            slug="test-blog-category", 
            )

    def test_if_url_exist(self):
        response = self.client.get('/blog/category/test-blog-category/')
        self.assertEqual(response.status_code, 200)

    def test_if_view_uses_right_template(self):
        response = self.client.get('/blog/category/test-blog-category/')
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, 'pages/category_list.html')

# shell

$ python manage.py test blog_tutorial.main.tests.test_views

Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.........../Users/highcenoid/blog_tutorial/env/lib/python3.8/site-packages/django/views/generic/list.py:86: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <class 'blog_tutorial.main.models.Project'> QuerySet.
  return self.paginator_class(
..........
----------------------------------------------------------------------
Ran 21 tests in 3.923s

OK
Destroying test database for alias 'default'...
```

In checking the list of blogs and projects, we create posts and projects by referencing it with a variable. We ran a loop to have a list of blog posts and projects that we can reference. We first checked if the blog and project url will equal a status_code of 200. We checked if the url can be specified by the name we set in the urls.py. Lastly, we checked if the blog and project list view use the right template for posts and projects.

Next, we checked if the pagination we declared in the Post and Project Detail View asserted to True on the pagination's 2nd page. We specified assertTrue if the blog and project list's post list will return the integer we set. 

In checking the post and project detail views, we start with creating objects and then testing if the url returns a status_code=200. Then we try if the template will produce the right template for the post and project detail views.

Finally, the category view is somewhat the same as what we did in the blog and project views since we are referencing its url to a slug, I decided not to take the test as long as the blog and project list view test and just made sure that the url and template returned true upon testing.
