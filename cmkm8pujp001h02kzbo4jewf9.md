---
title: "Problem 10: Duplicate Removal"
datePublished: Tue Jan 20 2026 06:55:55 GMT+0000 (Coordinated Universal Time)
cuid: cmkm8pujp001h02kzbo4jewf9
slug: problem-10-duplicate-removal
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1768892133388/353c478f-09a4-4f0a-9883-e55b5aecb7a4.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1768892140793/32bc5e55-8a12-4b25-94c7-719320f6e1a4.png
tags: python, learning, beginners

---

Hey everyone! 👋

Today, we're tackling a list manipulation problem: **Removing Duplicates while Maintaining Order**.

## **The Problem**

The goal is to write a function that removes duplicates from a list while maintaining the original order of elements.

**Example:**

* `remove_duplicates([1, 2, 2, 3, 1, 4])` should return `[1, 2, 3, 4]`
    

## **The Solution**

Here is the Python implementation:

```python
def remove_duplicates(lst):
    """
    Removes duplicates from a list while maintaining order.
    """
    seen = set()
    result = []

    for i in lst:
        if i not in seen:
            seen.add(i)
            result.append(i)
    return result

# Test case
print(remove_duplicates([1, 2, 2, 3, 1, 4]))
# Output: [1, 2, 3, 4]
```

## **Code Breakdown**

Let's walk through the code line by line:

1. `def remove_duplicates(lst):`
    
    * Defines a function named `remove_duplicates` that takes one parameter `lst` (a list).
        
2. `seen = set()`
    
    * Creates an empty set called `seen` to keep track of unique elements we've encountered. Using a set allows for efficient O(1) lookups.
        
3. `result = []`
    
    * Creates an empty list called `result` to store the final list without duplicates.
        
4. `for i in lst:`
    
    * Starts a loop that iterates through each element in the input list `lst`.
        
5. `if i not in seen:`
    
    * Checks if the current element `i` is not in the `seen` set (i.e., it's the first time we're seeing this element).
        
6. `seen.add(i)`
    
    * If the element is not in `seen`, adds it to the set to mark it as seen.
        
7. `result.append(i)`
    
    * Appends the element to the `result` list to maintain the order of first occurrence.
        
8. `return result`
    
    * Returns the final list with duplicates removed, in the order of their first appearance.
        

### **Example Walkthrough with** `[1, 2, 2, 3, 1, 4]`

1. `i = 1`: Not in seen → add to seen and result
    
    * seen: `{1}`
        
    * result: `[1]`
        
2. `i = 2`: Not in seen → add to seen and result
    
    * seen: `{1, 2}`
        
    * result: `[1, 2]`
        
3. `i = 2`: Already in seen → skip
    
4. `i = 3`: Not in seen → add to seen and result
    
    * seen: `{1, 2, 3}`
        
    * result: `[1, 2, 3]`
        
5. `i = 1`: Already in seen → skip
    
6. `i = 4`: Not in seen → add to seen and result
    
    * seen: `{1, 2, 3, 4}`
        
    * result: `[1, 2, 3, 4]`
        

**Final result:** `[1, 2, 3, 4]`

---

Happy coding! 💻