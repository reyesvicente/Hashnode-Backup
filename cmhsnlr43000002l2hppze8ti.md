---
title: "Problem 1: Sum of Digits: A Beginner's Guide to String Iteration in Python"
datePublished: Mon Nov 10 2025 04:40:08 GMT+0000 (Coordinated Universal Time)
cuid: cmhsnlr43000002l2hppze8ti
slug: problem-1-sum-of-digits-a-beginners-guide-to-string-iteration-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762749566534/d58c9eba-f0d3-4220-84e3-a3b89887a365.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1762749573625/29092e89-a2ae-40ed-b0d0-8822d6c5ac2a.png
tags: python, learning, beginners

---

When you're learning to code, one of the first problems you'll encounter is calculating the sum of individual digits within a number. It seems simple on the surface: take 123, add 1 + 2 + 3, and you get 6. But how do you actually make a computer do this? The answer introduces you to some fundamental Python concepts that you'll use throughout your programming journey.

I initially thought about using the `split()` method but later learned that the `split()` method only removes spaces and commas.

## The Problem

Let's say we want to write a function that takes any integer and returns the sum of its digits. For instance:

* `sum_of_digits(123)` should return 6
    
* `sum_of_digits(9999)` should return 36
    

This might seem tricky at first because numbers are mathematical objects, not sequences we can easily break apart. The key insight is converting the number into a string—something we can iterate over character by character.

## The Solution

Here's an elegant solution in Python:

```python
def sum_of_digits(n):
    total = 0
    for digit in str(n):
        total += int(digit)
    return total

print(sum_of_digits(144))  # Output: 9
print(sum_of_digits(9999)) # Output: 36
```

## How It Works

Let's break down this code step by step:

**Step 1: Initialize a Counter**

```python
total = 0
```

We start by creating a variable called `total` to keep track of our running sum. It begins at zero, and we'll add to it as we process each digit.

**Step 2: Convert to String and Iterate**

```python
for digit in str(n):
```

This is where the magic happens. The `str(n)` function converts our number into a string. When you convert 123 to a string, you get `'123'`—a sequence of characters. Python can iterate over strings character by character, so this loop will process each digit separately: first `'1'`, then `'2'`, then `'3'`.

**Step 3: Convert Back and Accumulate**

```python
total += int(digit)
```

For each character (which represents a digit), we use `int(digit)` to convert it back from a string to an actual integer. We then add it to our running total using the `+=` operator.

**Step 4: Return the Result**

```python
return total
```

Once we've processed all digits, we return the final sum.

## Why We Don't Need `split()`

You might wonder: why not use `split()` to break apart the digits? This is a common question for beginners, and it's worth addressing.

The `split()` method is designed to break strings into pieces based on a delimiter—usually whitespace, commas, or some other separator. When we convert a number to a string using `str(n)`, we don't have any delimiters between the digits. What we have is already a sequence of characters that we can iterate over directly.

For example, with `n = 123`:

* `str(123)` gives us `'123'`
    
* We can iterate directly over this: `'1'`, `'2'`, `'3'`
    
* Using `split()` would actually be unnecessary and less efficient
    

The string is already a sequence in Python, so we can loop through it without any additional parsing.

## The Python Way

The solution shown above is what Pythonic code looks like: clear, concise, and efficient. It leverages Python's built-in strengths—string iteration and type conversion—to solve the problem elegantly. This same pattern of converting numbers to strings for digit manipulation appears frequently in real-world programming, so mastering it now will serve you well.

## Try It Yourself

Want to test this? Run the code and try different numbers:

```python
print(sum_of_digits(144))  # 1 + 4 + 4 = 9
print(sum_of_digits(9999)) # 9 + 9 + 9 + 9 = 36
print(sum_of_digits(42))   # 4 + 2 = 6
```

The beauty of this approach is that it works for any integer you throw at it, regardless of how many digits it has. It's a small function, but it teaches you something important: sometimes the simplest solution comes from thinking about how to transform your data into a form that's easier to work with.