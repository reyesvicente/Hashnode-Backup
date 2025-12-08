---
title: "Flatten a Nested List"
seoTitle: "Flatten a Nested List"
seoDescription: "I know I've been a bit quiet lately. I actually came down with a pretty bad flu last week, which completely knocked me out. 🤒"
datePublished: Mon Dec 08 2025 10:54:58 GMT+0000 (Coordinated Universal Time)
cuid: cmix1bn7t000802kv4uon2fny
slug: flatten-a-nested-list
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765191262122/1ca1b35a-b63d-4150-9fd3-f16eb9a87f96.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1765191268470/43dfe38a-b045-41dd-aac7-c900a1f0cea2.png
tags: python, learning, beginners

---

Hey everyone! 👋

I know I've been a bit quiet lately. I actually came down with a pretty bad flu last week, which completely knocked me out. 🤒 That's why I missed posting about the coding challenges. I'm finally feeling a bit better and ready to get back into the swing of things!

Today, we're tackling a classic problem: **Flattening a Nested List**.

## The Problem

The goal is to write a function that takes a list which might contain other lists (nested to any depth) and converts it into a single, one-dimensional list.

**Examples:**

* `flatten([[1, 2], [3, 4], [5, 6]])` should return `[1, 2, 3, 4, 5, 6]`
    
* `flatten([1, [2, 3], [[4, 5], 6]])` should return `[1, 2, 3, 4, 5, 6]`
    

## The Solution

Here is the Python implementation using recursion. Recursion is perfect here because we don't know how deep the nesting goes!

```python
def flatten(nested_list):
    """
    Flattens a nested list into a single-level list.
    """
    result = []
    for item in nested_list:
        if isinstance(item, list):
            flattened_sublist = flatten(item)  # Recursively flatten the sublist
            result.extend(flattened_sublist)
        else:
            result.append(item)
    return result

# Test cases
print(flatten([[1, 2], [3, 4], [5, 6]]))
# Output: [1, 2, 3, 4, 5, 6]

print(flatten([1, [2, 3], [[4, 5], 6]]))
# Output: [1, 2, 3, 4, 5, 6]
```

## Code Breakdown

Let's walk through the code line by line to understand exactly what's happening.

1. `def flatten(nested_list):`
    
    * Defines a function called `flatten` that takes `nested_list` as input.
        
2. `result = []`
    
    * Initializes an empty list called `result`. This will store our final flattened elements.
        
3. `for item in nested_list:`
    
    * Begins iterating through each element in the input `nested_list`.
        
4. `if isinstance(item, list):`
    
    * Checks if the current `item` is a list using `isinstance()`. This is the crucial step that detects nested lists.
        
5. `flattened_sublist = flatten(item)`
    
    * If the item *is* a list, we recursively call `flatten()` on it. This handles the "nesting" by treating that sublist as its own problem to solve.
        
6. `result.extend(flattened_sublist)`
    
    * Since `flatten()` returns a list, we use `.extend()` to add all those individual elements to our main `result` list. If we used `.append()`, we'd just end up with another list inside our result!
        
7. `else:`
    
    * `result.append(item)`
        
    * If the item is *not* a list (it's just a number, string, etc.), we directly add it to `result`.
        
8. `return result`
    
    * Finally, returns the fully processed, flat list.
        

The beauty of this function is its flexibility. Thanks to recursion, it can handle a list nested 2 levels deep or 200 levels deep with the exact same logic!

---

Thanks for sticking with me while I recovered! I'll be catching up on more challenges soon. Happy coding! 💻