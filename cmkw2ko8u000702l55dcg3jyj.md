---
title: "Problem 11: Count Character Frequency"
datePublished: Tue Jan 27 2026 04:01:38 GMT+0000 (Coordinated Universal Time)
cuid: cmkw2ko8u000702l55dcg3jyj
slug: problem-11-count-character-frequency
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769486479188/097434a3-3f22-4c9e-b927-259e65aad21e.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1769486475097/75a1e642-7aab-4664-ae3e-30ac52dde40d.png
tags: python, learning, beginners

---

Hey everyone! 👋

Today, we're tackling a string manipulation problem: **Counting Character Frquency**.

## **The Problem**

The goal is to write a function that returns a dictionary with the frequency of each character in a string. The function should be case-insensitive and ignore spaces.

**Example:**

* `char_frequency("hello world")` should return `{'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}`
    

## **The Solution**

Here is the Python implementation:

```python
def char_frequency(s):
    """
    Returns a dictionary with the frequency of each character in a string.
    Ignores spaces and converts to lowercase.
    """
    freq_dict = {}
    s = s.lower().replace(" ", "")

    for char in s:
        if char in freq_dict:
            freq_dict[char] += 1
        else:
            freq_dict[char] = 1
    return freq_dict

# Test case
print(char_frequency("hello world"))
# Output: {'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}
```

## **Code Breakdown**

Let's walk through the code line by line:

1. `def char_frequency(s):`
    
    * Defines a function named `char_frequency` that takes one parameter `s` (a string).
        
2. `freq_dict = {}`
    
    * Creates an empty dictionary called `freq_dict` to store the characters and their counts.
        
3. `s = s.lower().replace(" ", "")`
    
    * Converts the input string `s` to lowercase using `.lower()` to ensure case-insensitivity.
        
    * Removes all spaces from the string using `.replace(" ", "")` so they aren't counted.
        
4. `for char in s:`
    
    * Starts a loop that iterates through each character in the processed string `s`.
        
5. `if char in freq_dict:`
    
    * Checks if the current character `char` is already a key in the `freq_dict`.
        
6. `freq_dict[char] += 1`
    
    * If the character is already in the dictionary, increments its count by 1.
        
7. `else: freq_dict[char] = 1`
    
    * If the character is not in the dictionary (it's the first time we've seen it), adds it to the dictionary with a count of 1.
        
8. `return freq_dict`
    
    * Returns the final dictionary containing the character frequencies.
        

### **Example Walkthrough with** `"hello world"`

1. **Preprocessing:**
    
    * Input: `"hello world"`
        
    * Lowercase & Remove Spaces: `"helloworld"`
        
2. **Iteration:**
    
    * `char = 'h'`: Not in dict → `freq_dict = {'h': 1}`
        
    * `char = 'e'`: Not in dict → `freq_dict = {'h': 1, 'e': 1}`
        
    * `char = 'l'`: Not in dict → `freq_dict = {'h': 1, 'e': 1, 'l': 1}`
        
    * `char = 'l'`: In dict → `freq_dict = {'h': 1, 'e': 1, 'l': 2}`
        
    * `char = 'o'`: Not in dict → `freq_dict = {'h': 1, 'e': 1, 'l': 2, 'o': 1}`
        
    * `char = 'w'`: Not in dict → `freq_dict = {'h': 1, 'e': 1, 'l': 2, 'o': 1, 'w': 1}`
        
    * `char = 'o'`: In dict → `freq_dict = {'h': 1, 'e': 1, 'l': 2, 'o': 2, 'w': 1}`
        
    * `char = 'r'`: Not in dict → `freq_dict = {'h': 1, 'e': 1, 'l': 2, 'o': 2, 'w': 1, 'r': 1}`
        
    * `char = 'l'`: In dict → `freq_dict = {'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1}`
        
    * `char = 'd'`: Not in dict → `freq_dict = {'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}`
        

**Final result:** `{'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}`

---

Happy coding! 💻