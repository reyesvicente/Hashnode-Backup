---
title: "How I'm preparing for a new job"
datePublished: Sun Mar 07 2021 11:33:14 GMT+0000 (Coordinated Universal Time)
cuid: cklz2u6jm063cers1da33gz2u
slug: how-im-preparing-for-a-new-job
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1615116702262/j_1yKlzEA.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1615116714293/VfNHfckiV.jpeg
tags: career

---

*<span>Photo by <a href="https://unsplash.com/@austindistel?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Austin Distel</a> on <a href="https://unsplash.com/s/photos/new-job?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>*

![](https://s3.amazonaws.com/revue/items/images/008/064/949/original/conrtas.gif?1615110592)

Preparing for a new job with zero knowledge of your new company's industry is scary, especially if it already has thousands of customers without a working platform, a non-functioning web and mobile application, and missing repositories. Fortunately, I was able to find the issue on both the web and mobile applications in their Amazon Web Services account. However, some functions are still not working. The previous developer was using Java (the last time I wrote code in Java was in 2008 or 2009 in college), so my brave self decided to re-write the whole application using Django and Django Rest Framework. 


![wew.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615116368933/2U-b8d_N7.png)

Here's how I'm preparing for the job

1. Testing new django-packages - the first django-package I tried was [django-cryptography](https://django-cryptography.readthedocs.io/) which encrypts data in the database, not in the built-in django admin. Another django-package I tested was [django-userena-ce](https://django-userena-ce.readthedocs.io/en/latest/#) which supplies the signup, sign-in, account management, privacy settings, and private messaging features.

2. Publishing a new article on [how to design better models in Django](https://vgr.icvn.tech/designing-better-models-in-django). Don't get me wrong. My main side project - [Indiependent.to](https://indiependent.to?utm_source=vgr) currently has three user types that I implemented with [Daniel Feldroy's tutorial](https://www.youtube.com/watch?v=V3zRZ6XRols). I was trying to find a different way to implement multiple user types and found django-packages for it. Still, [this article walk-through](https://simpleisbetterthancomplex.com/tutorial/2018/01/18/how-to-implement-multiple-user-types-with-django.html) was pretty neat and included a very informative flowchart.

3. Learning from podcasts - I stumbled upon two podcasts about the lessons on developing and scaling a Django app for HIPAA-compliant companies.

4. On giving project updates  - TBH, I'm not great at updating clients on a project's status - big or small. The problem is I always assume they understand industry-specific jargon or terms. It's a good thing I [found a structure](https://jacobian.org/2021/mar/5/exec-status-update/) on how to update and what key points to provide investors, executives, and board members that only lasts a few minutes.


Resources:

- https://jacobian.org/2021/mar/5/exec-status-update/
- https://simpleisbetterthancomplex.com/tutorial/2018/01/18/how-to-implement-multiple-user-types-with-django.html
- https://www.youtube.com/watch?v=V3zRZ6XRols
- https://vgr.icvn.tech/designing-better-models-in-django
- https://django-userena-ce.readthedocs.io/en/latest/#
- https://django-cryptography.readthedocs.io/
