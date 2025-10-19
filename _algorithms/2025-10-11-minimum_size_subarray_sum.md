---
layout: single
title: "209 - Minimum Size Subarray Sum"
date: 2025-10-11 00:00:00 +0000
categories: [Algorithms]
tags:
  - Sliding window
---

Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)


## Clues
1. The given data is positive ints, if you increase the pointer from right side, the sum will increase, if you increase the left pointer, the total sum will decrease.

## Summary
We are finding a case that is bigger than target first, increasing the right pointer. Once we've found a case, then we will move the left pointer to the right, so the window gets smaller. if the length's sum(right - left + 1) is smaller than the target, expand the window by increasing the right pointer.
if the target is already met, then move the left pointer to the right so as to find shortest length still meeting the condition.



### Time Complexity
- **Best Case:** O(N)
- **Average Case:** O(N)
- **Worst Case:** 

### Space Complexity
- O(1) for iterative approach, I didn't any data structure, sorted it in place.

```javascript

function minSubArrayLen(target, nums) {
  let left = 0;
  let sum = 0;
  let ans = Infinity;
  for (let right = 0; right < nums.length; right++) {
    sum += nums[right];

    while (sum >= target) {
      ans = Math.min(ans, right - left + 1);
      sum -= nums[left];
      left++;
    }
  }
  return ans === Infinity ? 0 : ans;
}
```



```javascript
var minSubArrayLen = function (target, nums) {
    let left = 0;
    let right = 0;
    const length = nums.length;
    let sum = nums[0];
    let min_length = Infinity
    while (right < length && left <= right) {
        if (sum >= target) {
            min_length = Math.min(right - left + 1, min_length)

            sum -= nums[left] // the order of these two lines is important
            left += 1; // the order of these two lines is important
            continue;
        } else {
            right++; // the order of these two lines is important
            sum += nums[right] // the order of these two lines is important

        }

    }
    return min_length == Infinity ? 0 : min_length;
};
```


## Test cases
```javascript
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Input: target = 4, nums = [1,4,4]
Input: target = 7, nums = [2,3,1,2,4,3]
```
