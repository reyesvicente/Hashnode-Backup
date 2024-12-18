---
title: "Getting started with Django Rest Framework"
datePublished: Sat Apr 08 2023 03:46:13 GMT+0000 (Coordinated Universal Time)
cuid: clg7fop2e000309mo2u8lewk0
slug: getting-started-with-django-rest-framework
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680869288511/f26f8738-de92-49f8-8945-b0d436213a55.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1680869259750/76332079-fc64-4431-a65e-e3f55f234719.jpeg
tags: python, django, django-rest-framework

---

*Photo by Markus Spiske:* [*https://www.pexels.com/photo/a-laptop-screen-with-text-4439901/*](https://www.pexels.com/photo/a-laptop-screen-with-text-4439901/)

In this article, we'll create a project that posts about rants using Django and Django Rest Framework. We'll use Django's built-in `slugify` method and override its `save()` method to automatically create a **slug** for each rant. We'll also use a third-party package called [drf-writable-nested](https://pypi.org/project/drf-writable-nested/) to handle the serialization of a `ManyToManyField` in the model.

We'll start by creating a virtual environment, installing the initial packages, creating the django project, creating the django app and finally doing the initial migrations

```shell
python -m venv venv
. venv/bin/activate
python -m pip install django djangorestframework
django-admin startproject myproject
cd myproject
python -m manage startapp rants
python -m manage migrate
```

We list `rest_framework` and our rants app in the INSTALLED\_APPS settings of our project and include them in the project [urls.py](http://urls.py)

```python
# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rants',
]

# myproject/urls.py
from django.urls import path, include

urlpatterns = [
    path('', include('rants.urls', namespace='main')),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework')),
]
```

Next we create the models inside the `rants` app:

```python
# models.py
from django.db import models


class Category(models.Model):
    title = models.CharField(max_length=50)
    slug = models.SlugField(max_length=50)

    def __str__(self):
        return self.title


class Rant(models.Model):
    title = models.CharField(max_length=150)
    slug = models.SlugField(max_length=150)
    categories = models.ManyToManyField(
        Category, related_name='rants_categories')

    class Meta:
        verbose_name = "rant"
        verbose_name_plural = 'rants'

    def __str__(self):
        return self.title
```

We have a Rant model with a title, slug with a `CharField` and a categories field with a `ManyToManyField` connected to a Category model with a title and a slug field.

Then we migrate the database `python -m manage makemigrations && python -m manage migrate`. Next, we create a serializer for both models:

```python
# serializers.py
from rest_framework import serializers
from .models import Rant, Category


class CategorySerializer(serializers.ModelSerializer):
    slug = serializers.SlugField(read_only=True)

    class Meta:
        model = Category
        fields = "__all__"


class RantSerializer(serializers.ModelSerializer):
    categories = CategorySerializer(many=True)
    slug = serializers.SlugField(read_only=True)


    class Meta:
        model = Rant
        fields = ('id', 'title', 'slug', 'categories')
        many = True
```

Finally, we create our views and map it to our urls so we can see the API endpoints of our app.

```python
# views.py
from rest_framework.response import Response
from rest_framework.generics import ListCreateAPIView, UpdateAPIView, DestroyAPIView

from .models import Rant
from .serializers import RantSerializer

class RantList(ListCreateAPIView):
    queryset = Rant.objects.all()
    serializer_class = RantSerializer

    def list(self, request):
        queryset = self.get_queryset()
        serializer = RantSerializer(queryset, many=True)
        return Response(serializer.data)


class RantUpdate(UpdateAPIView):
    queryset = Rant.objects.all()
    serializer_class = RantSerializer


class RantDelete(DestroyAPIView):
    queryset = Rant.objects.all()
    serializer_class = RantSerializer
```

We use `ListCreateAPIView` for read-write endpoints to represent a collection of model instances which provide a `get` and `post` method handler, `UpdateAPIView` for update-only endpoints of a single model instance which provides a `put` and `patch` method handler, `DestroyAPIView` for a delete-only endpoint of a single model instance which provides a `delete` method handler. Let's map these views to the [`urls.py`](http://urls.py)

```python
# urls.py
from django.urls import path

from .views import RantList, RantUpdate, RantDelete
from .models import Rant
from .serializers import RantSerializer

app_name = 'rants'


urlpatterns = [
    path('api/rants/', RantList.as_view(queryset=Rant.objects.all(), serializer_class=RantSerializer)),
    path('api/rants/update/<int:pk>/', RantUpdate.as_view(queryset=Rant.objects.all(), serializer_class=RantSerializer)),
    path('api/rants/delete/<int:pk>/', RantDelete.as_view(queryset=Rant.objects.all(), serializer_class=RantSerializer)),
]
```

We can now view the api endpoints in the browser using the drf package but I personally prefer to see the api endpoints using another package which is the `drf-yasg` package. Let's install and configure the package:

```python
python -m pip install drf-yasg

# settings.py
INSTALLED_APPS = [
    ....
    'rest_framework',
    'drf_yasg',
]
# urls.py
from django.urls import path, include
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
   openapi.Info(
      title="Rants API",
      default_version='v1',
      description="Rants",
      terms_of_service="https://www.google.com/policies/terms/",
      contact=openapi.Contact(email="contact@snippets.local"),
      license=openapi.License(name="BSD License"),
   ),
   public=True,
   permission_classes=[permissions.AllowAny],
)
urlpatterns = [path('api/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),]
```

Now we run `python -m manage runserver` and head over to [http://localhost:8000/api/](http://localhost:8000/api/) to see what we've created

![Rants API screenshot](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0xrydop6rj3hkm8uch38.png align="left")

But we have a problem, the categories field has an error despite inputting a `str` in the field.

```json
{
  "categories": [
    "Expected a list of items but got type \"str\"."
  ]
}
```

To solve this we'll install `drf-nested-writable` in our app to serialize the categories field then update the [`serializers.py`](http://serializers.py) file to include `WritableNestedModelSerializer` in our `RantSerializer`

```python
python -m pip install drf-nested-writable

# serializers.py
from drf_writable_nested.serializers import WritableNestedModelSerializer


class RantSerializer(WritableNestedModelSerializer):
    categories = CategorySerializer(many=True)
    slug = serializers.SlugField(read_only=True)

    class Meta:
        model = Rant
        fields = ('id', 'title', 'slug', 'categories')
        many = True
```

Now when we add new data for our app using the `rants_create` endpoint, we won't get the error we got above anymore but instead. We also override the `save()` method in our models for the `slug` field to automatically fill the database in

![Rants API](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/oxh8ghck248wzleajm24.png align="left")

The code to override the `save()` method in our models

```python
# models.py
...
from django.utils.text import slugify

...
    def save(self, *args, **kwargs):
        self.slug = slugify(self.title)
        super(Category, self).save(*args, **kwargs)
        return self.slug

...
    def save(self, *args, **kwargs):
        self.slug = slugify(self.title)
        super(Rant, self).save(*args, **kwargs)
        return self.slug
...
```

Conclusion: This took me a while to solve since I'm at GMT+8 but hey, at least it got solved and I learned how to override the save method in the models and learned about the drf-writable-nested package to fix my issue.