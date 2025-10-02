---
layout: single
title: "594. Longest Harmonious Subsequence"
date: 2025-10-02 00:00:00 +0000
categories: [Algorithms]
tags:
  - Sliding window
  - Two pointers
---

## Problem Description



We define a harmonious array as an array where the difference between its maximum value and its minimum value is exactly 1.

Given an integer array nums, return the length of its longest harmonious subsequence among all its possible subsequences.  

https://leetcode.com/problems/longest-harmonious-subsequence/description/?envType=problem-list-v2&envId=sliding-window

## Clues

1. diff(arr[max], arr[min]) = 1 => make it adjacent! (sorting)
2. longest subsequence => You can skip quite many cases while you are building current the longest one.


### Time Complexity
- **Best Case:** O(N)
- **Average Case:** 
- **Worst Case:** 

### Space Complexity
- O(1) for iterative approach, I didn't any data structure, sorted it in place.

## My Implementation

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findLHS = function(nums) {
    let max_length = 0;
    nums.sort((a,b)=> a-b)
    let i=0, j = 0
    j++;
    while(j < nums.length){
        if(nums[i] == nums[j]){
            j++
            continue;
        }
        else if(nums[j] - nums[i]==0){
            j++
            continue;
        }else if(nums[j] - nums[i] == 1) {
            max_length = Math.max(max_length, j - i + 1)
            j++;
            continue;
        }else{
            i++;
            continue
        }
    }
    return max_length
};
```

## Better Implementation
```javascript
var findLHS = function(nums) {
    nums.sort((a, b) => a - b);
    let j = 0, maxLength = 0;

    for (let i = 0; i < nums.length; i++) {
        while (nums[i] - nums[j] > 1) j++;
        if (nums[i] - nums[j] === 1) {
            maxLength = Math.max(maxLength, i - j + 1);
        }
    }

    return maxLength;
};
```

## Conclusion

This is a sample algorithm post demonstrating the structure for algorithm-related content.
