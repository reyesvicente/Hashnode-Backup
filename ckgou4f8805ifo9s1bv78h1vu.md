---
title: "Creating the blog project"
datePublished: Sun Oct 25 2020 08:11:59 GMT+0000 (Coordinated Universal Time)
cuid: ckgou4f8805ifo9s1bv78h1vu
slug: creating-the-blog-project
canonical: https://learnetto.com/tutorials/part-ii-creating-the-django-blog-project
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682767844245/458ca433-e5b3-4c00-b04a-8e989de2f029.jpeg
tags: tutorial, django, python3, bootstrap-4, beginners

---

Since this isn't a tutorial focused on the Cookiecutter-Django framework, I won't be getting too much into it. I'll only include how I fell in love with the framework when I found out what features it had that I struggled with implementing on the usual way to create a Django project. Some features included are Twitter bootstrap, django all-auth, sending emails using Anymail, media files storage using AWS S3, or GCP. It also comes with the custom user and supports Docker.

https://github.com/pydanny/cookiecutter-django

Open your terminal. Make sure you install cookiecutter on your machine. Otherwise, you'll run into an error.

`$ pip install "cookiecutter>=1.7.0"`

Now run this command:

`$ cookiecutter https://github.com/pydanny/cookiecutter-django`

You'll be asked for some values when you run the command. Be sure to run through the docs to avoid repeating this step.

https://cookiecutter-django.readthedocs.io/en/latest/project-generation-options.html

```bash
You've downloaded /Users/user/.cookiecutters/cookiecutter-django before. Is it okay to delete and re-download it? [yes]: yes
project_name [My Awesome Project]: Blog Tutorial
project_slug [blog_tutorial]: blog_tutorial
description [Behold My Awesome Project!]: Learn to jumpstart a production-ready blog using the Cookiecutter-Django framework and how to deploy it to Heroku
author_name [Vicente G. Reyes]: Vicente G. Reyes
domain_name [vgreyes.com]: vgreyes.com
email [hi@vgreyes.com]: highcenbugtv@vgreyes.com
version [0.1.0]: 0.1.0
Select open_source_license:
1 - MIT
2 - BSD
3 - GPLv3
4 - Apache Software License 2.0
5 - Not open source
Choose from 1, 2, 3, 4, 5 [1]: 1
timezone [UTC]: UTC
windows [n]: n
use_pycharm [n]: n
use_docker [n]: n
Select postgresql_version:
1 - 12.3
2 - 11.8
3 - 10.8
4 - 9.6
5 - 9.5
Choose from 1, 2, 3, 4, 5 [1]: 1
Select js_task_runner:
1 - None
2 - Gulp
Choose from 1, 2 [1]: 1
Select cloud_provider:
1 - AWS
2 - GCP
3 - None
Choose from 1, 2, 3 [1]: 1
Select mail_service:
1 - Mailgun
2 - Amazon SES
3 - Mailjet
4 - Mandrill
5 - Postmark
6 - Sendgrid
7 - SendinBlue
8 - SparkPost
9 - Other SMTP
Choose from 1, 2, 3, 4, 5, 6, 7, 8, 9 [1]: 1
use_async [n]: n
use_drf [n]: n
custom_bootstrap_compilation [n]: n
use_compressor [n]: n
use_celery [n]: n
use_mailhog [n]: n
use_sentry [n]: n
use_whitenoise [n]: y
use_heroku [n]: y
Select ci_tool:
1 - None
2 - Travis
3 - Gitlab
Choose from 1, 2, 3 [1]: 1
keep_local_envs_in_vcs [y]: y
debug [n]: y
 [SUCCESS]: Project initialized, keep up the good work!
```

Now change the directory into the project slug and try to get familiar with the folder structure. We'll be initializing a git repo inside the directory, committing our code, and pushing it to GitHub for version control. We're also creating a different environment for our blog using virtualenv to isolate our app's python project dependencies. Make sure you install the latest version found in https://pypi.org/project/virtualenv/. I'm using a mac, so my apologies for windows users if I don't execute windows commands in this tutorial.

```bash
$ cd blog_tutorial/
$ ls
$ git init
$ git add .
$ git commit -m "initial commit"
$ git remote add origin https://github.com/reyesvicente/blog_tutorial.git
$ git push -u origin master
```

Now that we've pushed our code to GitHub, let's create a virtual environment, activate it, and install the default packages included in the Cookiecutter-Django framework.

To install virtualenv, open our terminal, then execute:

`$ python -m pip install --user virtualenv`

Once the package is installed, check if you've installed the correct version by running:

`$ virtualenv --version 0:08:50 virtualenv 20.0.27 from /Users/user/.local/lib/python3.8/site-packages/virtualenv/__init__.py`

Now create the virtual environment by running:

`$ virtualenv env # name of the virtual environment is env`

Output:

```bash
created virtual environment CPython3.8.2.final.0-64 in 6874ms
  creator CPython3Posix(dest=/Users/user/blog_tutorial/env, clear=False, global=False)
  seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, via=copy, app_data_dir=/Users/user/Library/Application Support/virtualenv/seed-app-data/v1.0.1)
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
```

Once the virtual environment is created, let's activate the virtual environment and install the project dependencies. We'll also create a database using the createdb command, and we'll set the environment variables for our database. Make sure you have it on your machine if not, you can download it via Homebrew Cask or go to the Postgres App website and download it.

```bash
$ source env/bin/activate
$ python -m pip install -r requirements/local.txt
$ createdb blog_tutorial -U postgres --password <your_password>
$ export DATABASE_URL=postgres://postgres:<enter_your_psql_password>@127.0.0.1:5432/blog_tutorial
```

As of writing, the virtualenv package is at version 20.0.27.

You might be wondering why we need to install a virtual environment and isolate our project dependencies and install it globally. The reason is, if you have multiple projects in your development environment and you installed the project dependencies globally, there's a big chance that your app would crash. Let's get a little more info on that.

Imagine you have 2 Django projects. One uses a Django version 2.2.14, and the other uses Django 3.0.8. Both have different project dependencies that also have different versions in it.

In situations like this, it's a good practice to create separate virtual environments for the two projects since all Django projects are different, using other python packages.