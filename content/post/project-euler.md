---
title: "Project Euler Problem 1"
date: 2020-03-22T21:04:23+01:00
draft: true
description: "Showcasing the solutions for Project Euler 001 problem."
tags: [
    "project-euler",
    "practice",
]
categories: [
    "project-euler",
]
math: true
---
Showcasing the solutions for Project Euler 001 problem.
<!--more-->

## Introduction

I've recently discovered a really awesome way of practicing coding online, [Project Euler](https://projecteuler.net/about), a collection of computational problems itching to be solved. The are more than 700 problems available to solve with increasing in difficulty, and a new one is added each week. The first 250 is available on [HackerRank](https://www.hackerrank.com/contests/projecteuler/challenges), which is a good place to start solving them, with a nice in-browser code editor and predefined test cases to test your code against. By solving these challenges, you become a better developer since you have to consider the complexity of your algorithm, and the challenges force you to find the optimal way of solving the problem. Because if the algorithm isn't optimized, some of the test cases will fail. So if you haven't heard of Project Euler before, definitely check it out and solve some of the problems and see for yourself. I can guarentee you will meet some really eye-opening problems on how to write effiecent and optimal code. In fact, the [Problem 1](https://www.hackerrank.com/contests/projecteuler/challenges/euler001/problem) is already one of those eye-opening challenges, which I give my take on in this article, so before reading anymore, give it a go and try solving it and come back if you get stuck.

## Problem

The problem is simple to understand, find all multiples of 3 and 5 up to `N`, an arbitary number, then add all the multiples together. The example given on HackerRank is if `N` equals **10**, the multiples are 3, 5, 6, and 9. The sum of these numbers is **23**.

## Trivial solution

The trivial solution solving this problem is to iterate through all intergers starting from 3 to N and checking if its either a multiple of 3 or 5. If it is, then add it to the list of multipes and then sum up the list and get the answer. In fact, this could be done in one line in `Python`.

```python
def solution(n):
    return sum([i for i in range(3, n) if i % 3 == 0 or i % 5 == 0])
```

Now this takes up both linear space and time complexity, since you iterate through a loop and use a list to store multipes. The solution of course is correct, but if you run this on HackerRank some of the test cases fail, because of a Runtime error, meaning the algorithm could not finish in a reasonable amount of time. Now if you don't want to show off your Pythonic prowess by trying to solve the problem using a one-liner function, instead of storing the multiples in a list, you can immediately add it to a variable.

```python
def solution(n):
    sum = 0
    for i in range(3, n):
        if i % 3 == 0 or i % 5 == 0:
            sum += i
    return sum
```

However, this only helps by minimizing the space complexity to O(1), and the problem with the test case was with the time complexity, which is still linear. And for large integers running this algorithm will take a while and results in a Runtime error.

But anything less than a linear complexity is O(1) complexity, meaning it is done without a loop.

## Optimal Solution

After going through the discussion section for the problem on HackerRank and doing some research online, I've found the optimal way of solving this problem is using the [arithmetic series formula](https://www.mathwords.com/a/arithmetic_series.htm).

$$
f(x) = \int_{-\infty}^\infty\hat f(\xi)\,e^{2 \pi i \xi x}\,d\xi
$$

```python
def arithmetic_series(i, n):
    multiple = (n - 1) // i
    a1 = i
    an = i * multiple
    return multiple * (a1 + an) // 2

def solution(n):
    return arithmetic_series(3, n) + arithmetic_series(5, n) 
            - arithmetic_series(15, n)
```