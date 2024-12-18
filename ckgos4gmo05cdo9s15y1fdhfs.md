---
title: "Using the update_attribute() API using the rails console"
datePublished: Mon Aug 03 2020 20:47:10 GMT+0000 (Coordinated Universal Time)
cuid: ckgos4gmo05cdo9s15y1fdhfs
slug: using-the-updateattribute-api-using-the-rails-console-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682770691873/d7e4366c-b2cb-4297-9658-98314cb32ef4.png
tags: ruby

---

I had a task at work to add data from the rails console after generating migrations and was puzzled about how to do it. After spending some time on the problem, I asked for help and this is what I got:

```ruby
object=Model.first
object.description = "Lorem Ipsum"
object.save
```

The problem was, it returned `false` which I had a hint that it didn't work.

I checked the frontend and it didn't show the data I entered and ran reload on the rails console again but it still didn't update the data.

I knew I had to check the docs and figure out how to do it.

I found [this](https://stackoverflow.com/a/29747855) answer on SO which helped me with my problem.

```ruby
object=Model.first
object.update_attribute(:book_description, "Lorem Ipsum")
object.save
```

This updated the data on the frontend which was the goal of the task.