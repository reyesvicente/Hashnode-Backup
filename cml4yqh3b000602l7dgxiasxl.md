---
title: "Problem 12: Find Pairs with Target Sum"
datePublished: Mon Feb 02 2026 09:24:06 GMT+0000 (Coordinated Universal Time)
cuid: cml4yqh3b000602l7dgxiasxl
slug: problem-12-find-pairs-with-target-sum
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1770024181771/477b6693-9e23-4e2a-a2a5-07397f8b850a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1770024192064/e09882bb-5eef-4c4a-9ff8-c00d44b2cd8d.png
tags: python, learning, beginners

---

Hey everyone! 👋

Today, we're solving a classic coding interview problem: **Find Pairs with Target Sum**.

## **The Problem**

The goal is to write a function that finds all pairs of numbers in a list that add up to a specific target sum.

* The function should return a list of tuples.
    
* Each pair should appear only once (uniqueness).
    
* The order of elements in the tuple should be consistent (e.g., sorted).
    

**Example:**

* `find_pairs([1, 5, 7, -1, 5], 6)` should return `[(1, 5), (7, -1)]`
    
* `find_pairs([2, 3, 4, 5], 9)` should return `[(4, 5)]`
    

## **The Solution**

Here is the Python implementation:

```python
def find_pairs(lst, target):
    """
    Finds all unique pairs in the list that sum up to the target.
    """
    seen = set()
    pairs_found = set()
    for num in lst:
        complement = target - num
        if complement in seen:
            pairs_found.add((min(num, complement), max(num, complement)))
        seen.add(num)
    return list(pairs_found)

# Test cases
print(find_pairs([1, 5, 7, -1, 5], 6))
# Output: [(1, 5), (-1, 7)]
```

## **Code Breakdown**

Let's walk through the code line by line:

1. `def find_pairs(lst, target):`
    
    * Defines a function named `find_pairs` that takes a list of numbers `lst` and a target sum `target`.
        
2. `seen = set()`
    
    * Initializes an empty set called `seen` to keep track of numbers we have encountered so far. Sets are used for O(1) lookups.
        
3. `pairs_found = set()`
    
    * Initializes a set called `pairs_found` to store the valid pairs. Using a set ensures that we don't store duplicate pairs (e.g., if the same pair appears multiple times in the list).
        
4. `for num in lst:`
    
    * Iterates through each number `num` in the input list.
        
5. `complement = target - num`
    
    * Calculates the `complement`. This is the number we need to find in our `seen` set to make the `target` sum with the current `num`.
        
6. `if complement in seen:`
    
    * Checks if the `complement` exists in the `seen` set. If it does, it means we have found a pair (`num` + `complement` = `target`).
        
7. `pairs_found.add((min(num, complement), max(num, complement)))`
    
    * If a pair is found, we add it to `pairs_found`.
        
    * `min(num, complement), max(num, complement)` creates a tuple where the smaller number comes first. This normalization ensures that `(1, 5)` and `(5, 1)` are treated as the same pair and handled correctly by the set's uniqueness property.
        
8. `seen.add(num)`
    
    * Adds the current number `num` to the `seen` set so it can be used as a complement for future numbers.
        
9. `return list(pairs_found)`
    
    * Converts the set of pairs back into a list and returns it.
        

## **Example Walkthrough**

Let's trace the function with `find_pairs([1, 5, 7, -1, 5], 6)`:

1. **Initialization:** `seen = {}`, `pairs_found = {}`, `target = 6`
    
2. **Iteration 1:** `num = 1`
    
    * `complement = 6 - 1 = 5`
        
    * Is `5` in `seen`? No.
        
    * Add `1` to `seen`: `seen = {1}`
        
3. **Iteration 2:** `num = 5`
    
    * `complement = 6 - 5 = 1`
        
    * Is `1` in `seen`? **Yes**.
        
    * Add `(1, 5)` to `pairs_found`. `pairs_found = {(1, 5)}`
        
    * Add `5` to `seen`: `seen = {1, 5}`
        
4. **Iteration 3:** `num = 7`
    
    * `complement = 6 - 7 = -1`
        
    * Is `-1` in `seen`? No.
        
    * Add `7` to `seen`: `seen = {1, 5, 7}`
        
5. **Iteration 4:** `num = -1`
    
    * `complement = 6 - (-1) = 7`
        
    * Is `7` in `seen`? **Yes**.
        
    * Add `(-1, 7)` to `pairs_found`. `pairs_found = {(1, 5), (-1, 7)}`
        
    * Add `-1` to `seen`: `seen = {1, 5, 7, -1}`
        
6. **Iteration 5:** `num = 5`
    
    * `complement = 6 - 5 = 1`
        
    * Is `1` in `seen`? **Yes**.
        
    * Add `(1, 5)` to `pairs_found`. `pairs_found` is a set, so it remains `{(1, 5), (-1, 7)}`.
        
    * Add `5` to `seen`.
        

**Final Result:** `[(1, 5), (-1, 7)]` (Order in list may vary, but pairs are correct).

---

Happy coding! 💻