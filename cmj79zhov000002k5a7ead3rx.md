---
title: "Problem 5: Palindrome Checker"
datePublished: Mon Dec 15 2025 14:55:10 GMT+0000 (Coordinated Universal Time)
cuid: cmj79zhov000002k5a7ead3rx
slug: problem-5-palindrome-checker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765810478748/4796bd5e-07d2-40f7-9bc0-47f2283e3593.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1765810485707/ba43e3e4-e486-41cf-b626-25ca503739ef.png
tags: python, learning, begginers

---

Hey everyone! 👋

Continuing with our coding challenges series! Today we're looking at a classic string manipulation problem: checking for palindromes.

## **The Problem**

A palindrome is a word, phrase, number, or other sequence of characters that reads the same forward and backward (ignoring spaces, punctuation, and capitalization).

**Examples:**

* "racecar" -&gt; True
    
* "A man a plan a canal Panama" -&gt; True
    
* "hello" -&gt; False
    

## **The Solution**

Here is a clean Python solution that handles case insensitivity and ignores spaces:

```python
def is_palindrome(s):
    """
    Checks if a string is a palindrome.
    Ignores case and spaces.
    """
    s = str.lower(s)
    s = s.replace(" ", "")
    return s == s[::-1]

# Test cases
print(is_palindrome("wilabaliw"))
# Output: True
print(is_palindrome("A man a plan a canal Panama"))
# Output: True
```

## **Code Breakdown**

Let's break down how this works:

1. `s = str.lower(s)`
    
    * First, we convert the entire string to lowercase. This ensures that "Racecar" and "racecar" are treated as the same string.
        
2. `s = s.replace(" ", "")`
    
    * Next, we remove all spaces. This is important for phrases like "A man a plan..." where the spaces don't match up when reversed, but the letters do.
        
3. `return s == s[::-1]`
    
    * This is the Pythonic magic! `s[::-1]` creates a reversed copy of the string.
        
    * We simply compare the cleaned original string with its reverse. If they are equal, it returns `True`; otherwise, `False`.
        

Short, sweet, and effective!

---

Stay tuned for more challenges! Happy coding! 💻