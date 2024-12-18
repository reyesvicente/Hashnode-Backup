---
title: "Model Architecture Planning"
datePublished: Tue Oct 27 2020 08:24:14 GMT+0000 (Coordinated Universal Time)
cuid: ckgrx3a2c01mbz9s13b6f67mw
slug: model-architecture-planning
canonical: https://learnetto.com/tutorials/part-iv-model-architecture-planning-4eeba952-8c05-40ba-b551-7aee5c37a269
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682767739839/328244e5-5f76-4cd5-b04a-330de028219f.jpeg
tags: tutorial, django, python3, beginners

---

Django is based on the Model-View-Template software design architecture, which means that the Model takes care of our data and logic, the views will take care of what the users will see in the browser when you render your site, and the templates will take care of what we render as HTML, CSS, Javascript and our dynamic content.

It's good to visualize our models before defining them in our models.py for us to see how our model structure would be laid out. The figure below shows how we will determine the database of the blog we're building. 



![Models.py of the blog](https://cdn.hashnode.com/res/hashnode/image/upload/v1603799964500/rX1rrns8Z.png)
models.py
 


We've created the models for Post and Category. The figure we created also shows the relationship between both models including its multiplicities. According to moz://a are the numbers on the figure that shows the numbers (maximum and minimum) of each model that may be present in the relationship.- moz://a.

Now that we know ow we'll layout our models, let's start creating our project with the Cookiecutter-Django framework so we won't have too much of a hard time deploying our app in production.