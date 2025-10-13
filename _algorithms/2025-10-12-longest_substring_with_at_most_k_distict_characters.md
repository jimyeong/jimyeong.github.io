---
layout: single
title: "340 - Longest Substring with At Most K Distinct Characters"
date: 2025-10-02 00:00:00 +0000
categories: [Algorithms]
tags:
  - Sliding window
---

Given a string s and an integer k, return the length of the longest substring of s that contains at most k distinct characters.
```
EX)
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.

Input: s = "aa", k = 1
Output: 2
Explanation: The substring is "aa" with length 2.

```

[340. Longest Substring with At Most K District Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/description/?envType=problem-list-v2&envId=sliding-window)


## Clues
1. The problem is quite similar to [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/). The difference is instead of sum, you should use a counter to measure the length of the arr(right - left + 1)


## Approach
Well, I knew this would be similar to the previous quiz I resolved. but I didn't know how to count the alphabets. 
at first, I thought of using a Set, but I didn't know properly the method,
If I had known that I could use count.size, count.get. It would have been much easier.

For me, the common patterns i make silly approach is to try to implement from a-z. 
If a data structure can do a job, use it.


### Time Complexity
- **Best Case:** right traverses only once in the for loop.
the left pointer as well it traveres maximum once => O(N)
 

### Space Complexity
- O(1), the problem suggests strings so, 26characters will come in as an argument. and if duplicated, it will be less then 26chars. 


```javascript
var lengthOfLongestSubstringKDistinct = function (s, k) {
    if (k ==0 || s.length == 0) return 0;
    
    let left = 0;
    const length = s.length;
    const count = new Map();
    let max =0
    for(let right =0; right < length; right++){
        const ch = s[right];
        count.set(ch, (count.get(ch) || 0) + 1)

        while(count.size > k){
            
            //   max = Math.max(right - left + 1, max);
            count.set(s[left], count.get(s[left]) - 1);
            if(count.get(s[left])==0)count.delete(s[left])
            left++; 
        }
        max = Math.max(right - left + 1, max);
    }
    return max 

};
```




## Test cases
```javascript
s='eceba', k=2
s='aa', k=1
```
