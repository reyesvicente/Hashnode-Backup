---
title: "Problem 15: Longest Common Prefix"
datePublished: 2026-02-23T10:55:06.263Z
cuid: cmlz28e8j001927mngknmfmg8
slug: problem-15-longest-common-prefix
cover: https://cloudmate-test.s3.us-east-1.amazonaws.com/uploads/covers/5f95116240346172a86c20c6/52536222-3251-4247-85d0-a1def442c8c2.png
ogImage: https://cloudmate-test.s3.us-east-1.amazonaws.com/uploads/og-images/5f95116240346172a86c20c6/b4cf5944-d879-454e-9198-989564c3011e.png
tags: python, learning, beginners

---

Hey everyone! 👋

Today, we're solving a popular string manipulation problem: **Longest Common Prefix**.

## **The Problem**

The goal is to write a function that finds the longest common prefix among a list of strings.

*   A **prefix** is a substring that occurs at the beginning of a string.
    
*   The function should return the longest common string that all words in the list start with.
    
*   If there is no common prefix, it should return an empty string `""`.
    

**Examples:**

*   `longest_common_prefix(['flower', 'flow', 'flight'])` → Should return `'fl'`
    
*   `longest_common_prefix(['dog', 'racecar', 'car'])` → Should return `""` (no common prefix)
    
*   `longest_common_prefix(['interspecies', 'interstellar'])` → Should return `'inters'`
    

## **The Solution**

Here is the Python implementation:

```python
def longest_common_prefix(strings):
    """
    Finds the longest common prefix among a list of strings.
    """
    # Check for empty or invalid input
    if not strings:
        return ""

    # Set the first string as the initial prefix
    prefix = strings[0]

    # Iterate through the remaining strings
    for s in strings[1:]:
        # Shorten the prefix until it matches the start of the current string
        while not s.startswith(prefix):
            prefix = prefix[:-1]
            # If the prefix becomes empty, there is no common prefix
            if prefix == "":
                return ""

    return prefix

# Test cases
print(longest_common_prefix(['flower', 'flow', 'flight']))      # 'fl'
print(longest_common_prefix(['dog', 'racecar', 'car']))         # ''
print(longest_common_prefix(['interspecies', 'interstellar']))  # 'inters'
```

## **Code Breakdown**

Let's walk through the code line by line:

1.  `def longest_common_prefix(strings):`
    
    *   Defines a function named `longest_common_prefix` that takes a list of words (`strings`) as input.
        
2.  `if not strings:`
    
    *   Checks if the input list is empty or `None`.
        
    *   If it is empty, we return an empty string `""`.
        
    *   *Note: This is a safer and more Pythonic check than* `strings is None or strings == 0` *limit cases!*
        
3.  `prefix = strings[0]`
    
    *   Sets our initial assumption: let's assume the entire first string is the common prefix.
        
4.  `for s in strings[1:]:`
    
    *   Iterates through each subsequent string (`s`) in the list.
        
    *   `strings[1:]` is Python slice notation that means "all elements from index 1 to the end".
        
5.  `while not s.startswith(prefix):`
    
    *   Keeps running as long as the current string `s` doesn't start with our assumed `prefix`.
        
    *   `.startswith()` checks if a string begins with the specified prefix.
        
6.  `prefix = prefix[:-1]`
    
    *   Removes the last character from the prefix to make it shorter.
        
    *   `prefix[:-1]` is string slicing that takes all characters except the last one.
        
7.  `if prefix == "":`
    
    *   If we've removed all characters from the prefix, it means the current word shares no common letters at the beginning with our initial assumed prefix.
        
    *   We immediately return `""` as we know there is no common sequence.
        
8.  `return prefix`
    
    *   Once we've successfully compared our prefix against all strings, the remaining shortened string is safely our longest common prefix!
        

## **Example Walkthrough**

Let's trace the function with `longest_common_prefix(['flower', 'flow', 'flight'])`:

1.  **Initialization:**
    
    *   Start with `prefix = 'flower'`
        
2.  **Compare with** `'flow'` **(First iteration):**
    
    *   `'flow'` doesn't start with `'flower'`? True.
        
    *   Remove last char: `prefix = 'flowe'`
        
    *   `'flow'` doesn't start with `'flowe'`? True.
        
    *   Remove last char: `prefix = 'flow'`
        
    *   `'flow'` doesn't start with `'flow'`? False. Stop while loop.
        
    *   Current prefix is safely `'flow'`.
        
3.  **Compare with** `'flight'` **(Second iteration):**
    
    *   `'flight'` doesn't start with `'flow'`? True.
        
    *   Remove last char: `prefix = 'flo'`
        
    *   `'flight'` doesn't start with `'flo'`? True.
        
    *   Remove last char: `prefix = 'fl'`
        
    *   `'flight'` doesn't start with `'fl'`? False. Stop while loop.
        
    *   Current prefix is safely `'fl'`.
        
4.  **Result:** Loop completes, return `'fl'`. ✅
    

Now let's try `longest_common_prefix(['dog', 'racecar', 'car'])`:

1.  **Initialization:**
    
    *   Start with `prefix = 'dog'`
        
2.  **Compare with** `'racecar'`**:**
    
    *   `'racecar'` doesn't start with `'dog'`? True. Shorten to `prefix = 'do'`
        
    *   `'racecar'` doesn't start with `'do'`? True. Shorten to `prefix = 'd'`
        
    *   `'racecar'` doesn't start with `'d'`? True. Shorten to `prefix = ""`
        
3.  **Result:** Prefix is empty `""`. Immediate return `""`. ❌ No common prefix found.
    

## **Key Optimizations**

This approach relies heavily on **Horizontal Matching**:

*   **Shrinking Search Pattern:** Instead of checking letter by letter precisely (vertical scaling index-by-index), we start by assuming the maximum possible prefix and aggressively peel off letters from the end whenever mismatches are found.
    
*   **Early Escape:** If the prefix shrinks to an empty string at any point, we completely abort without iterating through the rest of the array strings.
    

This pattern is highly effective, easy to read, and simple to reason out! 🚀

* * *

Happy coding! 💻