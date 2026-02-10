---
title: "Problem 13: Group Anagrams"
datePublished: Tue Feb 10 2026 02:53:27 GMT+0000 (Coordinated Universal Time)
cuid: cmlg0axhn000002la5xb93rfw
slug: problem-13-group-anagrams
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1770691998031/e98d92f2-714e-4228-b7a3-6b87a6a7572f.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1770691992172/79f583af-0046-47cc-8a2a-22f041141846.png
tags: python, learning, beginners

---

Hey everyone! 👋

Today, we're tackling a popular interview question: **Group Anagrams**.

## **The Problem**

The goal is to write a function that groups words that are anagrams of each other from a given list of strings.

* An **anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
    
* The function should return a list of lists, where each sublist contains words that are anagrams.
    

**Example:**

* `group_anagrams(['listen', 'silent', 'hello', 'world', 'enlist'])`
    
    * Should return: `[['listen', 'silent', 'enlist'], ['hello'], ['world']]`
        

## **The Solution**

Here is the Python implementation:

```python
def group_anagrams(words):
    """
    Groups words that are anagrams of each other.
    """
    # Handle edge case: if input is None or empty list
    if words is None or len(words) == 0:
        return []

    anagram_groups = {}

    for word in words:
        # Create a key that is the same for all anagrams
        # 1. Lowercase to ensure case-insensitivity
        # 2. Sort the characters
        # 3. Convert to tuple to make it hashable (usable as a dict key)
        key = tuple(sorted(word.lower()))

        if key in anagram_groups:
            anagram_groups[key].append(word)
        else:
            anagram_groups[key] = [word]

    return list(anagram_groups.values())

# Test cases
print(group_anagrams(['listen', 'silent', 'hello', 'world', 'enlist']))
# Output: [['listen', 'silent', 'enlist'], ['hello'], ['world']]
```

## **Code Breakdown**

Let's walk through the code line by line:

1. `def group_anagrams(words):`
    
    * Defines a function named `group_anagrams` that takes a list of strings `words` as input.
        
2. `if words is None or len(words) == 0:`
    
    * Checks if the input list is `None` or empty. If so, it returns an empty list `[]`. This handles edge cases effectively.
        
3. `anagram_groups = {}`
    
    * Initializes an empty dictionary called `anagram_groups`.
        
    * **Key**: A sorted tuple of characters (unique signature for anagrams).
        
    * **Value**: A list of words that match that signature.
        
4. `for word in words:`
    
    * Iterates through each `word` in the input list.
        
5. `key = tuple(sorted(word.lower()))`
    
    * `word.lower()`: Converts the word to lowercase so 'Listen' and 'listen' are treated the same.
        
    * `sorted(...)`: Sorts the characters alphabetically. For example, 'listen' becomes `['e', 'i', 'l', 'n', 's', 't']`.
        
    * `tuple(...)`: Converts the sorted list into a tuple. This is crucial because lists are mutable and cannot be used as dictionary keys, but tuples are immutable and hashable.
        
6. `if key in anagram_groups:`
    
    * Checks if this sorted character tuple (the key) is already in our dictionary.
        
7. `anagram_groups[key].append(word)`
    
    * If the key exists, it means we've found another anagram for this group. We append the original `word` to the list associated with this key.
        
8. `else: anagram_groups[key] = [word]`
    
    * If the key does not exist, this is the first word of this anagram type we've encountered. We create a new entry in the dictionary with the key and a new list containing the current `word`.
        
9. `return list(anagram_groups.values())`
    
    * `anagram_groups.values()` retrieves all the lists of anagrams from the dictionary.
        
    * `list(...)` converts this view object into a standard list and returns it.
        

## **Example Walkthrough**

Let's trace the function with `['listen', 'silent', 'hello']`:

1. **Initialization:** `anagram_groups = {}`
    
2. **Word 1:** `'listen'`
    
    * `key`: `('e', 'i', 'l', 'n', 's', 't')`
        
    * Key not in map. Add it: `anagram_groups = {('e', 'i', 'l', 'n', 's', 't'): ['listen']}`
        
3. **Word 2:** `'silent'`
    
    * `key`: `('e', 'i', 'l', 'n', 's', 't')` (Same as 'listen'!)
        
    * Key exists. Append: `anagram_groups = {('e', 'i', 'l', 'n', 's', 't'): ['listen', 'silent']}`
        
4. **Word 3:** `'hello'`
    
    * `key`: `('e', 'h', 'l', 'l', 'o')`
        
    * Key not in map. Add it.
        

**Final Result:** `list(anagram_groups.values())` -&gt; `[['listen', 'silent'], ['hello']]`

---

Happy coding! 💻