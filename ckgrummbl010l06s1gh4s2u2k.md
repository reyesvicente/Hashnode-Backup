---
title: "Creating the app for the blog"
datePublished: Mon Oct 26 2020 07:51:12 GMT+0000 (Coordinated Universal Time)
cuid: ckgrummbl010l06s1gh4s2u2k
slug: creating-the-app-for-the-blog
canonical: https://learnetto.com/tutorials/part-iii-creating-the-app-for-the-blog
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682767798079/e9be20e8-f7ce-42e3-b84c-f8959cf45ae6.jpeg
tags: tutorial, django, python3, beginners

---

Now let's create a new Django application. We'll call our new app main.

`$ django-admin startapp main`

Once the app is created, we'll have to move the app in the blog\_tutorial directory together with the default users app and configure our apps.py, urls.py, and include the app in our base.py config. Let's now create those files and configure it for our app.

Our blog\_tutorial/main/apps.py is configured as follow:

```python
# blog_tutorial/main/apps.py
from django.apps import AppConfig
from django.utils.translation import gettext_lazy as _

class MainConfig(AppConfig):
    name = 'blog_tutorial.main'
    verbose_name = _("Main")

    def ready(self):
        try:
            pass
        except ImportError:
            pass
```

And our blog\_tutorial/main/urls.py has an empty urlpatterns variable. This is because we will be defining all our URLs in the config/urls.py file.

```bash
# blog_tutorial/main/urls.py
app_name = "main"
urlpatterns = [ ]
```

And don't forget to add our app to the LOCAL\_APPS in the config/settings/base.py

```python
# config/settings/base.py

...
LOCAL_APPS = [
    "blog_tutorial.users.apps.UsersConfig",
    # Your stuff: custom apps go here
    "blog_tutorial.main.apps.MainConfig",
]
```

Then let's apply our migrations and run our app.

```bash
$ python manage.py migrate

Operations to perform:
  Apply all migrations: account, admin, auth, contenttypes, sessions, sites, socialaccount, users
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying users.0001_initial... OK
  Applying account.0001_initial... OK
  Applying account.0002_email_max_length... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
  Applying sites.0001_initial... OK
  Applying sites.0002_alter_domain_unique... OK
  Applying sites.0003_set_site_domain_and_name... OK
  Applying socialaccount.0001_initial... OK
  Applying socialaccount.0002_token_max_lengths... OK
  Applying socialaccount.0003_extra_data_default_dict... OK


$ python manage.py runserver
Watching for file changes with StatReloader
INFO 2020-07-21 17:21:50,781 autoreload 66844 4497481152 Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
July 21, 2020 - 17:21:51
Django version 3.0.8, using settings 'config.settings.local'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Open your browser and head over https://127.0.0.1:800 or https://localhost:8000 to see what you just created using the Cookiecutter-Django framework.

Ok, bye. Thanks for listening to my Ted talk.

Kidding.

You can check the official docs of Cookiecutter-Django to get more information on the features of the framework. https://cookiecutter-django.readthedocs.io/en/latest/developing-locally.html

In this chapter, we've installed a virtual environment to separate our project in our development environment, install the default project dependencies of Cookiecutter-Django, created a PostgreSQL database, created a Django app and added the necessary config in our app and settings to make our Django blog work.