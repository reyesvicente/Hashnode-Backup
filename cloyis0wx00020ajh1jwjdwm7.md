---
title: "Understanding the Distinction: PUT vs. PATCH in API Design"
datePublished: Tue Nov 14 2023 16:00:12 GMT+0000 (Coordinated Universal Time)
cuid: cloyis0wx00020ajh1jwjdwm7
slug: understanding-the-distinction-put-vs-patch-in-api-design
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ehyV_XOZ4iA/upload/dba91c51001cd4c15a2aa7a95b00a08f.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1699967177329/828fbaac-b37a-47ec-926a-d6a22110cb40.avif
tags: python, apis

---

I got asked about the difference with `PUT` and `PATCH` in my interview earlier which made me write this article about the difference between the two.

In the context of APIs, both PATCH and PUT are HTTP methods used to update resources, but they are used in slightly different ways.

1. **PUT Method:**
    
    * The PUT method is used to update a resource or create a new resource if it doesn't exist at the specified URL.
        
    * When you use PUT to update a resource, you typically send the full representation of the resource in the request.
        
    * If the resource already exists, the entire resource is replaced with the new representation provided in the request.
        
    * If the resource does not exist, a new resource is created with the specified representation at the given URL.
        
    * PUT is idempotent, meaning that if the same request is made multiple times, it has the same effect as making it once.
        
    
    **Example in Python using requests:**
    

```python
import requests
import json

url = 'https://api.example.com/resource/123'
data = {'key': 'new_value'}

response = requests.put(url, data=json.dumps(data), headers={'Content-Type': 'application/json'})

print(response.status_code)
```

1. **PATCH Method:**
    

* The PATCH method is used to apply partial modifications to a resource.
    
* Instead of sending the full representation of the resource, you send only the changes you want to apply.
    
* PATCH is particularly useful when you want to update a resource without sending the entire payload, which can be more efficient.
    
* Like PUT, PATCH is idempotent.
    

**Example in Python using requests:**

```python
import requests
import json

url = 'https://api.example.com/resource/123'
data = {'key': 'new_value'}

response = requests.patch(url, data=json.dumps(data), headers={'Content-Type': 'application/json'})

print(response.status_code)
```

In summary, use PUT when you want to update a resource by providing the full representation, and use PATCH when you want to apply partial modifications to a resource. The choice between them depends on the use case and the level of granularity you need in your updates.