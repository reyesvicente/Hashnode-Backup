---
title: "Creating the data and fine-tuning the templates"
datePublished: Tue Oct 27 2020 08:49:07 GMT+0000 (Coordinated Universal Time)
cuid: ckgrxca2f01olz9s1f88pbkgd
slug: creating-the-data-and-fine-tuning-the-templates
canonical: https://learnetto.com/tutorials/part-vii-creating-the-data-and-fine-tuning-the-templates
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682767530482/71c5d921-8d83-4f5f-b3c8-3d23543daca4.jpeg
tags: tutorial, django, python3, beginners

---

Django follows the DRY principle of software development or the Don't repeat yourself principle, aiming to minimize writing code or repeating code. [There's a discussion on the Portland Pattern Repository](https://wiki.c2.com/?DontRepeatYourself) that explains this in context. The argument made a more straightforward explanation that even 10-year-olds might understand: 

Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.
To implement this, we're creating a partial directory inside our templates directory and separating our navbar and footer. The templates directory now looks like this:
 
Current template directory
![Template Directory](https://cdn.hashnode.com/res/hashnode/image/upload/v1603800383639/gnx1VL6d2.png)

Your footer should have this:

``` html
# templates/partials/_footer.html
<footer class="py-4 bg-primary text-white-50">
    <div class="container text-center">
        <small>
            Copyright &copy;
            <a class="text-light" href="https://vgreyes.com">
                Heisenberg 2020
            </a>
        </small>
    </div>
</footer>
```

And your navbar would have this:

``` html
# tempaltes/partials/_navbar.html
<div class="mb-1">
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar-brand" href="{% url 'blog' %}">
                VGReyes
            </a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <a href="{% url 'blog' %}"><span class="navbar-toggler-icon"></a></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ml-auto">
                    <li {% if request.resolver_match.url_name == 'blog' %}class="nav-item active"{% else %}class="nav-item"{% endif %}>
                        <a class="nav-link" href="{% url 'blog' %}">Home</a>
                    </li>
                    <li {% if request.resolver_match.url_name == 'about' %}class="nav-item active"{% else %}class="nav-item"{% endif %}>
                        <a class="nav-link" href="{% url 'about' %}">About</a>
                    </li>
                    <li {% if request.resolver_match.url_name == 'portfolio' %}class="nav-item active"{% else %}class="nav-item"{% endif %}>
                        <a class="nav-link" href="{% url 'portfolio' %}">Projects</a>
                    </li>
                    <li {% if request.resolver_match.url_name == 'contact' %}class="nav-item active"{% else %}class="nav-item"{% endif %}>
                        <a class="nav-link" href="{% url 'contact' %}">Contact</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</div>
```

Its good practice to start the Django files in an underscore "_" to show it is difference from the other html files, other than of course having different file names. You'll also notice that we have an if-else tag wrapped in Django template language inside our list tags. This translates to an active bootstrap class when the request matches the url name named in the urls.py file.

Now let's go to our shell to create our data. Run this:

`$ python manage.py shell`

Let's start creating the blog posts of our app then the projects

``` bash
Python 3.8.2 (default, Jun 16 2020, 15:51:47)
Type 'copyright', 'credits' or 'license' for more information
IPython 7.16.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from blog_tutorial.main.models import Post

In [2]: from django.utils import timezone

In [3]: blog_post = Post.objects.create(title="First blog in Django", slug="first-blog-in-django"
   ...: , overview="Echo park squid jean shorts kitsch, af vaporware celiac. Fixie tbh meggings l
   ...: isticle distillery.", body="Listicle ramps you probably haven't heard of them tousled iro
   ...: ny etsy chia put a bird on it shaman. Gentrify glossier sustainable man braid. Squid clou
   ...: d bread biodiesel cliche chambray wolf marfa etsy austin jean shorts.", image="default.pn
   ...: g", created_on="2020-07-27", updated_on="", categories="blog", status="published")

In [4]: blog_post = Post.objects.create(title="Second blog in Django", slug="second-blog-in-djan
    ...: go", overview="Hexagon portland pok pok, succulents put a bird on it cornhole art party
    ...: banjo gentrify kitsch", body="iPhone air plant. Scenester woke snackwave butcher tattooe
    ...: d pug man bun hammock umami poke skateboard truffaut pour-over hell of. Hoodie food truc
    ...: k crucifix squid. Chicharrones skateboard paleo freegan unicorn lomo put a bird on it.",
    ...:  image="default.png", created_on=timezone.now(), updated_on=timezone.now(), status="publ
    ...: ished")

In [5]: blog_post = Post.objects.create(title="Third blog in Django", slug="third-blog-in-django
    ...: ", overview="Humblebrag cronut cloud bread. YOLO beard seitan", body="Tumeric hammock ad
    ...: aptogen letterpress deep v. Small batch migas pickled craft beer bitters listicle shaman
    ...:  iPhone live-edge af fingerstache.", image="default.png", created_on=timezone.now(), upd
    ...: ated_on=timezone.now(), status="published")

In [6]: blog_post = Post.objects.create(title="Woke roof party beard", slug="woke-roof-party-bea
    ...: rd", overview="humblebrag cronut cloud bread. YOLO beard seitan", body="Tumeric hammock
    ...: adaptogen letterpress deep v. Small batch migas pickled craft beer bitters listicle sham
    ...: an iPhone live-edge af fingerstache. Pinterest waistcoat bitters everyday carry quinoa",
    ...:  image="default.png", created_on=timezone.now(), updated_on=timezone.now(), status="publ
    ...: ished")
```

To check if the objects were created, we create a blog_posts variable for our Post objects.

``` bash
In [7]: blog_posts = Post.objects.all()

In [8]: print(blog_posts)
<QuerySet [<Post: First blog in Django>, <Post: Second blog in Django>, <Post: Third blog in Django>, <Post: Woke roof party beard>]>
Let's now create the Category and Project objects

In [9]: from blog_tutorial.main.models import Category

In [10]: blog_category = Category.objects.create(title="blog", slug="blog")

In [11]: blog_category = Category.objects.create(title="python", slug="python")

In [12]: blog_category = Category.objects.create(title="django", slug="django")

In [13]: blog_category = Category.objects.create(title="learnetto", slug="learnetto")

In [14]: from blog_tutorial.main.models import Project

In [15]: project = Project.objects.create(title="Acme", slug="acme", image="default.png", live_si
    ...: te="www.example.com", github_link="https://github.com/acme", description="A site for Acm
    ...: e")

In [16]: project = Project.objects.create(title="Learnetto", slug="learnetto", image="default.png
    ...: ", live_site="www.learnetto.com", github_link="https://github.com/learnetto", descriptio
    ...: n="A web app for Learnetto.")

In [17]: project = Project.objects.create(title="ICVN Tech Studio", slug="icvn-tech-studio", imag
    ...: e="default.png", live_site="www.icvntechstudio.co", github_link="https://github.com/icvn
    ...: techstudio", description="A static site for ICVN Tech Studio")

In [18]: blog_cat = Category.objects.all() 

In [19]: project_list = Projects.objects.all()

In [20]: project_list = Project.objects.all()

In [21]: print(blog_cat)
<QuerySet [<Category: blog>, <Category: python>, <Category: django>, <Category: learnetto>]>

In [22]: print(project_list)
<QuerySet [<Project: Acme>, <Project: Learnetto>, <Project: ICVN Tech Studio>]>
```

Now that we created the data. Let's show them on the template. 