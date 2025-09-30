---
layout: single
title: "Test Algorithm Post - Binary Search"
date: 2025-09-30 00:00:00 +0000
categories: [Algorithms]
tags:
  - Binary Search
  - Testing
  - Sample
---

## Problem Description

This is a test post for the Algorithms section demonstrating a simple binary search implementation.

## Approach

Binary search is an efficient algorithm for finding an item in a sorted array by repeatedly dividing the search interval in half.

### Time Complexity
- **Best Case:** O(1)
- **Average Case:** O(log n)
- **Worst Case:** O(log n)

### Space Complexity
- O(1) for iterative approach
- O(log n) for recursive approach (due to call stack)

## Implementation

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1

# Example usage
arr = [1, 3, 5, 7, 9, 11, 13, 15]
result = binary_search(arr, 7)
print(f"Element found at index: {result}")  # Output: 3
```

## Conclusion

This is a sample algorithm post demonstrating the structure for algorithm-related content.
