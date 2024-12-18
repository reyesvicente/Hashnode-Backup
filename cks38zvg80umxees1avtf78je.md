---
title: "Hacker Rank Challenge: Utopian Tree"
datePublished: Wed Apr 24 2019 10:40:02 GMT+0000 (Coordinated Universal Time)
cuid: cks38zvg80umxees1avtf78je
slug: hacker-rank-challenge-utopian-tree
canonical: https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1628429694817/CKd9QOeQm.png
tags: blog, python, algorithm

---

The Utopian Tree goes through _2_ cycles of growth every year. Each spring, it _doubles_ in height. Each summer, its height increases by _1_ meter.

Laura plants a Utopian Tree sapling with a height of _1_ meter at the onset of spring. How tall will her tree be after _**n**_ growth cycles?

For example, if the number of growth cycles is _**n = 5,**_the calculations are as follows:

    Period  Height
    0          1
    1          2
    2          3
    3          6
    4          7
    5          14

**Function Description**
========================

Complete the _utopianTree_ function in the editor below. It should return the integer height of the tree after the input number of growth cycles.

utopianTree has the following parameter(s):

*   _n_: an integer, the number of growth cycles to simulate

**Input Format**
================

The first line contains an integer, _**t,**_ the number of test cases.

_**t**_ subsequent lines each contain an integer, _**n,**_ denoting the number of cycles for that test case.

**Constraints**
===============

![Screen Shot 2019-04-24 at 6.29.11 PM](https://cdn.hashnode.com/res/hashnode/image/upload/v1628429693502/y0DzTYzNx.png)

**Output Format**
=================

For each test case, print the height of the Utopian Tree after _**n**_ cycles. Each height must be printed on a new line.

**Sample Input**
================

3
0
1
4

**Sample Output**
=================

1
2
7

**Explanation**
===============

There are _3_ test cases.
=========================

In the first case ( _**n  = 0**_ ), the initial height ( _**H = 1**_ ) of the tree remains unchanged.

In the second case ( _**n = 1**_ ), the tree doubles in height and is **2** meters tall after the spring cycle.

In the third case ( _**n = 4**_ ), the tree doubles its height in spring ( _**n = 1, H = 2**_ ), then grows a meter in summer ( **n = 2, H = 3** ), then doubles after the next spring ( **n = 3, H = 6** ), and grows another meter after summer ( **n = 4, H = 7** ). Thus, at the end of 4 cycles, its height is **7** meters.

Solution:
=========

import sys
T = int(sys.stdin.readline())
for \_ in range(T):
    N = int(sys.stdin.readline())
    height = 1
        
    for i in range(N):
        if i % 2 == 0:
            height \*= 2
        else:
            height += 1
            
    print(height)

### Share this:

*   [Facebook](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=facebook)
*   [LinkedIn](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=linkedin)
*   [Twitter](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=twitter)
*   [Reddit](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=reddit)
*   [Pinterest](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=pinterest)
*   [WhatsApp](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=jetpack-whatsapp)
*   [Telegram](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=telegram)
*   [Tumblr](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=tumblr)
*   [Email](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=email)
*   [More](https://highcenburg.tech.blog/#)

*   [Skype](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=skype)
*   [Print](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/#print)
*   [Pocket](https://highcenburg.tech.blog/2019/04/24/hacker-rank-challenge-utopian-tree/?share=pocket)

### Like this:

Like Loading...

### _Related_