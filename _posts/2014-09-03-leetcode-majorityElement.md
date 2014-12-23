---
layout: post
title:  "LeetCode-Majority Element"
keywords: "LeetCode, Majority Element"
description: "Sublime3中文输入法问题"
category: 编程
tags: [LeetCode]
---

{% include JB/setup %}

#### Majority Element
> Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.
>
>You may assume that the array is non-empty and the majority element always exist in the array.

#### 方法一: Moore voting algorithm
We maintain a current candidate and a counter initialized to 0. As we iterate the array, we look at the current element x:

1. If the counter is 0, we set the current candidate to x and the counter to 1.

2. If the counter is not 0, we increment or decrement the counter based on whether x is the current candidate.

After one pass, the current candidate is the majority element. Runtime complexity = O(n).

```c
int majorityElement(int a[], int num)
{
	int candidate = 0;
	int count = 0;
	int i;

	for(i = 0; i < num; i++)
	{
		if(count == 0)
		{
			candidate = a[i];
			count = 1;
		}
		else
		{
			if(candidate == a[i])
			{
				count++;
			}
			else
			{
				count--;
			}
		}
	}

	return candidate;
}
```
<!--break-->

#### 方法二: 先排序后查找

Find the longest contiguous identical element in the array after sorting.

```c++
int majorityElement(vector<int> &num)
{
	sort(num.begin(), num.end());
	return num[num.size() / 2];
}

```

#### 方法三: Hash Table

Maintain a hash table of the counts of each element, then find the most common one.