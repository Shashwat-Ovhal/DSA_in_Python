# 🚀 DSA Mastery — Python |

> A personal tracker for my 70-Day DSA journey in Python.  
> Every folder contains my notes, approaches, and LeetCode solutions — written in my own words.

---

## 📌 About This Repo

- **Language:** Python 3
- **Platform:** LeetCode
- **Goal:** Crack placement drives
- **Total Problems Target:** ~218 problems across 10 weeks
- **Roadmap Source:** 70 Days DSA Roadmap ([Whimsical](https://whimsical.com/70-days-dsa-roadmap-7hvrJ3vHZtK9Pu5MDGRbRc))

---

## 🗺️ Full Roadmap Overview

### 🟣 Phase 1 — Pure Foundation (Weeks 1–4)

| Week | Topic | Problems | Status |
|------|-------|----------|--------|
| Week 1 | Arrays & Two-Pointer Techniques | ~25 | 🔄 In Progress |
| Week 2 | Strings + Hashing | ~28 | ⏳ Upcoming |
| Week 3 | Linked List + Stack + Queue | ~22 | ⏳ Upcoming |
| Week 4 | Binary Search + Basic Recursion | ~22 | ⏳ Upcoming |

### 🔵 Phase 2 — Core DSA (Weeks 5–8)

| Week | Topic | Problems | Status |
|------|-------|----------|--------|
| Week 5 | Trees & Hierarchical Traversal | ~22 | ⏳ Upcoming |
| Week 6 | BST + Priority Queue (Heaps) | ~18 | ⏳ Upcoming |
| Week 7 | Graph Traversal | ~23 | ⏳ Upcoming |
| Week 8 | Greedy + Interval Problems | ~14 | ⏳ Upcoming |

### 🟢 Phase 3 — Advanced Problem Solving (Weeks 9–11)

| Week | Topic | Problems | Status |
|------|-------|----------|--------|
| Week 9 | Dynamic Programming (1D) | ~20 | ⏳ Upcoming |
| Week 10 | Advanced DP + Backtracking (2D) | ~24 | ⏳ Upcoming |
| Week 11+ | Mock Interviews + Revision | 30 re-solves + 8–10 mocks | ⏳ Upcoming |

---

## 📁 Repo Structure
```
DSA-Python-Placement/
│
├── README.md
│
├── Phase1_Foundation/
│   ├── Week1_Arrays_TwoPointer/
│   │   ├── notes.md
│   │   ├── topic1_arrays_prefix/
│   │   ├── topic2_two_pointer/
│   │   ├── topic3_sliding_window/
│   │   └── topic4_complexity_practice/
│   ├── Week2_Strings_Hashing/
│   ├── Week3_LinkedList_Stack_Queue/
│   └── Week4_BinarySearch_Recursion/
│
├── Phase2_CoreDSA/
│   ├── Week5_Trees/
│   ├── Week6_BST_Heaps/
│   ├── Week7_Graphs/
│   └── Week8_Greedy_Intervals/
│
└── Phase3_Advanced/
    ├── Week9_DP_1D/
    ├── Week10_DP_2D_Backtracking/
    └── Week11_MockInterviews/
```

---

## ✅ Week 1 — Arrays & Two-Pointer Techniques

> **Phase:** 1 — Pure Foundation  
> **Duration:** Days 1–7  
> **Total Problems:** 28  
> **Daily Target:** 5–6 problems/day × 5 days + 2 days revision/buffer

### 📋 Patterns Covered
- Time & Space Complexity (deep understanding)
- Arrays — prefix, suffix, difference array
- Two pointer technique
- Sliding window basics

### 🗓️ Suggested Daily Order

| Day | Focus | Problems |
|-----|-------|----------|
| Day 1 | Complexity warm-up (Topic 4) | LC #1, #217, #169, #75, #287 |
| Day 2 | Prefix & suffix arrays (Topic 1 — Part A) | LC #1480, #724, #303, #238 |
| Day 3 | Difference array + prefix wrap-up (Topic 1 — Part B) | LC #121, #560, #1109, #1094 |
| Day 4 | Two pointer technique (Topic 2) | LC #167, #125, #283, #26, #15, #11 |
| Day 5 | Two pointer hard + Sliding window start (Topic 2 + 3) | LC #18, #42, #643, #209 |
| Day 6 | Sliding window (Topic 3 wrap-up) | LC #1493, #1456, #1438, #1004, #239 |
| Day 7 | Revision + re-solve 5 hardest problems | Pick your weakest 5 |

---

### 📦 Topic 1 — Arrays (Prefix, Suffix, Difference Array)

> **Target:** 8 problems | Easy: 3 | Medium: 4

| # | Problem | Difficulty | LeetCode | My Solution | Notes |
|---|---------|------------|----------|-------------|-------|
| 1 | Best Time to Buy and Sell Stock | 🟢 Easy | [LC #121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | | |
| 2 | Running Sum of 1d Array | 🟢 Easy | [LC #1480](https://leetcode.com/problems/running-sum-of-1d-array/) | | |
| 3 | Find Pivot Index | 🟢 Easy | [LC #724](https://leetcode.com/problems/find-pivot-index/) | | |
| 4 | Product of Array Except Self | 🟡 Medium | [LC #238](https://leetcode.com/problems/product-of-array-except-self/) | | |
| 5 | Subarray Sum Equals K | 🟡 Medium | [LC #560](https://leetcode.com/problems/subarray-sum-equals-k/) | | |
| 6 | Range Sum Query — Immutable | 🟢 Easy | [LC #303](https://leetcode.com/problems/range-sum-query-immutable/) | | |
| 7 | Corporate Flight Bookings | 🟡 Medium | [LC #1109](https://leetcode.com/problems/corporate-flight-bookings/) | | |
| 8 | Car Pooling | 🟡 Medium | [LC #1094](https://leetcode.com/problems/car-pooling/) | | |

**Key Concept:** Prefix sum formula → `prefix[i] = prefix[i-1] + arr[i]`  
**Key Concept:** Difference array for range updates → `diff[l] += val`, `diff[r+1] -= val`, then take prefix sum.

---

### 📦 Topic 2 — Two Pointer Technique

> **Target:** 8 problems | Easy: 4 | Medium: 3 | Hard: 1

| # | Problem | Difficulty | LeetCode | My Solution | Notes |
|---|---------|------------|----------|-------------|-------|
| 1 | Two Sum II — Input Array is Sorted | 🟢 Easy | [LC #167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | | |
| 2 | Valid Palindrome | 🟢 Easy | [LC #125](https://leetcode.com/problems/valid-palindrome/) | | |
| 3 | Move Zeroes | 🟢 Easy | [LC #283](https://leetcode.com/problems/move-zeroes/) | | |
| 4 | Remove Duplicates from Sorted Array | 🟢 Easy | [LC #26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | | |
| 5 | 3Sum | 🟡 Medium | [LC #15](https://leetcode.com/problems/3sum/) | | |
| 6 | Container With Most Water | 🟡 Medium | [LC #11](https://leetcode.com/problems/container-with-most-water/) | | |
| 7 | 4Sum | 🟡 Medium | [LC #18](https://leetcode.com/problems/4sum/) | | |
| 8 | Trapping Rain Water | 🔴 Hard | [LC #42](https://leetcode.com/problems/trapping-rain-water/) | | |

**Key Concept:** Standard two pointer setup in Python:
```python
left = 0
right = len(arr) - 1
while left < right:
    # process
    left += 1   # or right -= 1
```
**Note:** Trapping Rain Water (LC #42) is the hardest problem this week. Don't panic if it takes 2 attempts. Understand the left_max/right_max approach first.

---

### 📦 Topic 3 — Sliding Window Basics

> **Target:** 7 problems | Easy: 1 | Medium: 5 | Hard: 1

| # | Problem | Difficulty | LeetCode | My Solution | Notes |
|---|---------|------------|----------|-------------|-------|
| 1 | Maximum Average Subarray I | 🟢 Easy | [LC #643](https://leetcode.com/problems/maximum-average-subarray-i/) | | |
| 2 | Longest Subarray of 1s After Deleting One Element | 🟡 Medium | [LC #1493](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/) | | |
| 3 | Maximum Number of Vowels in Substring of Length K | 🟡 Medium | [LC #1456](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/) | | |
| 4 | Minimum Size Subarray Sum | 🟡 Medium | [LC #209](https://leetcode.com/problems/minimum-size-subarray-sum/) | | |
| 5 | Longest Continuous Subarray With Absolute Diff ≤ Limit | 🟡 Medium | [LC #1438](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/) | | |
| 6 | Max Consecutive Ones III | 🟡 Medium | [LC #1004](https://leetcode.com/problems/max-consecutive-ones-iii/) | | |
| 7 | Sliding Window Maximum | 🔴 Hard | [LC #239](https://leetcode.com/problems/sliding-window-maximum/) | | |

**Key Concept:** Fixed window → move both ends together. Variable window → expand right, shrink left when condition breaks.  
**Key Concept:** `collections.deque` is essential for LC #239 — use it as a monotonic deque.

---

### 📦 Topic 4 — Time & Space Complexity (Applied Practice)

> **Target:** 5 problems | Easy: 3 | Medium: 2  
> *Solve these first on Day 1 as warm-up — focus on analysing Big-O for each solution.*

| # | Problem | Difficulty | LeetCode | My Solution | Notes |
|---|---------|------------|----------|-------------|-------|
| 1 | Two Sum | 🟢 Easy | [LC #1](https://leetcode.com/problems/two-sum/) | | |
| 2 | Contains Duplicate | 🟢 Easy | [LC #217](https://leetcode.com/problems/contains-duplicate/) | | |
| 3 | Majority Element | 🟢 Easy | [LC #169](https://leetcode.com/problems/majority-element/) | | |
| 4 | Sort Colors | 🟡 Medium | [LC #75](https://leetcode.com/problems/sort-colors/) | | |
| 5 | Find the Duplicate Number | 🟡 Medium | [LC #287](https://leetcode.com/problems/find-the-duplicate-number/) | | |

**Key Concept:** For every problem this week, write the time and space complexity in your notes before submitting. Make it a habit.


---

## 📈 Overall Progress Tracker

| Phase | Week | Problems Solved | Target | Status |
|-------|------|----------------|--------|--------|
| Phase 1 | Week 1 | 0 / 28 | 28 | 🔄 In Progress |
| Phase 1 | Week 2 | 0 / 28 | 28 | ⏳ |
| Phase 1 | Week 3 | 0 / 22 | 22 | ⏳ |
| Phase 1 | Week 4 | 0 / 22 | 22 | ⏳ |
| Phase 2 | Week 5 | 0 / 22 | 22 | ⏳ |
| Phase 2 | Week 6 | 0 / 18 | 18 | ⏳ |
| Phase 2 | Week 7 | 0 / 23 | 23 | ⏳ |
| Phase 2 | Week 8 | 0 / 14 | 14 | ⏳ |
| Phase 3 | Week 9 | 0 / 20 | 20 | ⏳ |
| Phase 3 | Week 10 | 0 / 24 | 24 | ⏳ |
| Phase 3 | Week 11+ | 0 / 30 | 30 | ⏳ |
| | **Total** | **0 / 251** | **251** | |

---

## 📝 How I Use This Repo

Each topic folder contains:
- `notes.md` — concept explanations in my own words
- Each solution file has a header comment with: problem link, approach, time complexity, space complexity

**Solution file template:**
```python
# Problem: <Problem Name>
# Link: https://leetcode.com/problems/<slug>/
# Difficulty: Easy / Medium / Hard
# Approach: <one line description>
# Time Complexity: O(?)
# Space Complexity: O(?)

class Solution:
    def methodName(self, ...):
        pass
```

---

### 📊 Week 1 Summary

| Metric | Value |
|--------|-------|
| Total problems | 28 |
| Easy | 11 |
| Medium | 15 |
| Hard | 2 |
| Topics covered | 4 |
| Daily avg target | 5–6 problems |
---

*Started: [29/03/2026] | Target completion: 70 days*
