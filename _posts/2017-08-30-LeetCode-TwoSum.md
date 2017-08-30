---
layout: post
title:  "TwoSum Python"
date:  2017-08-30
categories: LeetCode Python
tags: TwoSum
---

* content
{:toc}



TwoSum 

```
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        for i in range(0,len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i] + nums[j] == target:
                    return [i,j]
```
