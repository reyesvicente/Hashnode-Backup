---
title: "Problem 7: Factorial"
datePublished: Mon Dec 29 2025 07:26:13 GMT+0000 (Coordinated Universal Time)
cuid: cmjqu42fe000602l7dy6vewfw
slug: problem-7-factorial
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766993159445/6ddc4279-caab-4059-bc06-4418a606db3a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1766993154731/1440429b-502c-494e-bcc5-9990eb96fa8f.png
tags: python, learning, beginners

---

Hey everyone! 👋

Today, we're tackling a classic mathematical problem: **Calculating the Factorial**.

## **The Problem**

The goal is to write a function that calculates the factorial of a number.  
The factorial of a non-negative integer `n`, denoted by `n!`, is the product of all positive integers less than or equal to `n`.

**Formula:** `n! = n × (n-1) × (n-2) × ... × 1`  
**Special Case:** `0! = 1`

**Example:**

* `factorial(5)` should return `120` (because `5 × 4 × 3 × 2 × 1 = 120`)
    
* `factorial(0)` should return `1`
    

## **The Solution**

Here is the Python implementation using a loop:

```plaintext
def factorial(n):
    """
    Calculates the factorial of a number using a loop.
    """
    # Base case: 0! and 1! are both 1
    if n == 0 or n == 1:
        return 1
    else:
        result = 1
        # Loop from 2 up to n
        for i in range(2, n + 1):
            result *= i
        return result

# Test cases
print(factorial(5))
# Output: 120
print(factorial(0))
# Output: 1
print(factorial(7))
# Output: 5040
```

## **Code Breakdown**

Let's walk through the code line by line:

1. `def factorial(n):`
    
    * Defines a function named `factorial` that takes one parameter `n` (the number to calculate the factorial for).
        
2. `if n == 0 or n == 1: return 1`
    
    * **Base case:** If `n` is `0` or `1`, we return `1` immediately.
        
    * This is because `0!` is defined as `1`, and `1!` is obviously `1`.
        
3. `else: result = 1`
    
    * If `n` is greater than `1`, we initialize a variable `result` to `1`.
        
    * `1` is the multiplicative identity (multiplying by 1 doesn't change the value), making it a safe starting point.
        
4. `for i in range(2, n + 1):`
    
    * We start a loop that iterates from `2` up to `n`.
        
    * `range(2, n + 1)` generates numbers starting from `2` and stopping *before* `n + 1`, which means it includes `n`.
        
5. `result *= i`
    
    * In each iteration, we multiply the current `result` by the loop variable `i`.
        
    * This accumulates the product (`1 * 2 * 3 * ... * n`).
        
6. `return result`
    
    * After the loop finishes, we return the final computed factorial value.
        

### **Example Walkthrough with** `factorial(5)`

Let's trace what happens when we call `factorial(5)`:

1. **Check:** Is `5` equal to `0` or `1`? No.
    
2. **Initialize:** `result` starts at `1`.
    
3. **Loop:** `i` will take values `2`, `3`, `4`, `5`.
    
    * **i = 2**: `result` = 1 \* 2 = **2**
        
    * **i = 3**: `result` = 2 \* 3 = **6**
        
    * **i = 4**: `result` = 6 \* 4 = **24**
        
    * **i = 5**: `result` = 24 \* 5 = **120**
        
4. **Return:** The loop ends, and the function returns `120`.
    

---

Happy coding! 💻