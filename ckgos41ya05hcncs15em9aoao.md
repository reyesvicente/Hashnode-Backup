---
title: "Creating a blog with Cookiecutter-Django & deploying it to Heroku: Introduction"
datePublished: Sat Oct 24 2020 04:42:10 GMT+0000 (Coordinated Universal Time)
cuid: ckgos41ya05hcncs15em9aoao
slug: creating-a-blog-with-cookiecutter-django-and-deploying-it-to-heroku-introduction-1
canonical: https://learnetto.com/tutorials/creating-a-blog-with-cookiecutter-django-deploying-it-to-heroku
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1609337180330/wyO2ZeIqr.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1609337189111/y-XkEw1l0.jpeg
tags: tutorial, django, bootstrap-4, beginners, heroku

---

*<span>Photo by <a href="https://unsplash.com/@lucabravo?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Luca Bravo</a> on <a href="https://unsplash.com/s/photos/coding-tutorial?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>*

# Table of Contents

I. An introduction on the tech stack we're using and how we'd style our blog

II. Creating the blog project

III. Starting a Django App

IV. Model Architecture Planning

V. Creating the models.py, views.py, urls.py, admin.py & the superuser

VI. Testing our app using Unittests

VII. Creating the data and fine-tuning the templates

VIII. Showing Data on the frontend

IX. Deployment to Heroku

## I. Introduction

In this tutorial, I'll walk you through developing a blog using the Cookiecutter-Django framework, storing the projects' static assets in an AWS S3 bucket, and deploying the blog to Heroku. Our blog would have a blog model that we'll call Post, a categories model, a contact form, and a model to showcase our past works or a portfolio page.

We'll then be using [Bootswatch](https://bootswatch.com/darkly/)'s Darkly theme, built on top of the Bootstrap CSS framework for our blog's styles. Feel free to choose a different theme if you prefer to.

#### Homepage

![Homepage](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610195658/D7oEgQ5eB.png)

#### About page

![About page](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610197614/ec81Q35Vk.png)

#### Blog detail page

![Blog detail page](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610199603/zzkYz98ME.png)

#### Portfolio page

![Portfolio page](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610202321/6gkIZvDsL.png)

#### Contact page

![Contact page](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610204190/9E8Zlghp0.png)


Since we now know how the site would look, let's start creating our models and understand how Django's Model-View-Template architecture works.