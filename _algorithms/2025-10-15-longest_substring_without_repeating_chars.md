---
layout: single
title: "3 - Longest Substring Without Repeating Characters"
date: 2025-10-02 00:00:00 +0000
categories: [Algorithms]
tags:
  - Sliding window
---

Given a string s, find the length of the longest substring without duplicate characters.
```
EX)
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.

Input: s = "aa", k = 1
Output: 2
Explanation: The substring is "aa" with length 2.

```

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)


## Pattern Insight 🧰
**Pattern:** Sliding Window — Distinct Character Tracking  
**Trigger:**  
- string or array 
- “Find the length of the longest contiguous subarray (or substring) that satisfies a given condition.”, 
- There shouldn't be any duplication in window 

### Time Complexity


### Space Complexity

### Edge Cases





```javascript
var lengthOfLongestSubstring = function(s) {
    const length = s.length;
    if(length < 2) return length;
    let left = 0;
    let max = -Infinity;
    const seen = new Map()
    for(let right = 0; right  < length; right++){
        const ch = s[right];

        if(seen.has(ch) && seen.get(ch) >= left){
            left = seen.get(ch) + 1
        }
        seen.set(ch, right) 
        max = Math.max(right - left + 1, max)
    }
    return max == -Infinity ? 0 : max;
};
```

**Core Idea:**  
- Right pointer expansion
- You can jump to avoid having duplicated chars in the window with Map<char, index>


