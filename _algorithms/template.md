# 🧠 Longest Substring with At Most K Distinct Characters

> ⏱ 1 min read · 🟡 Medium · #SlidingWindow #TwoPointers

---

## 📌 Problem Summary

| 항목              | 내용                                               |
|-------------------|----------------------------------------------------|
| **Platform**      | LeetCode #340                                     |
| **Difficulty**    | Medium                                            |
| **Category**      | Sliding Window, Hash Map                          |
| **Time Taken**    | 12 min                                            |
| **Language**      | JavaScript                                        |
| **Date**          | 2025-10-12                                        |

---

## 💡 Approach

- 처음엔 단순한 이중 반복문으로 접근했지만, 시간 복잡도가 `O(n²)`이라 비효율적임을 확인했습니다.  
- Sliding Window를 사용해 윈도우 내 **서로 다른 문자 수**를 실시간으로 추적하고, `k`를 초과할 경우 `left` 포인터를 이동시켜 윈도우를 축소하는 방식으로 해결했습니다.

---

## 👉 Key Insight

> **윈도우 내의 ‘distinct character count’를 유지하면서 조건을 만족할 때마다 길이를 갱신한다.**

---

## 📝 Problem Description

> Given a string `s` and an integer `k`, return the length of the longest substring of `s` that contains at most `k` distinct characters.

**Example**  
```text
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.