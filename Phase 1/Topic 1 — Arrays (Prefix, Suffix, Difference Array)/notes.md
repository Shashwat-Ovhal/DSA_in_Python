# DSA Notes — Topic 2: Arrays
### Prefix Sum · Suffix Product · Difference Array

---

## 1. Best Time to Buy and Sell Stock — LC #121 | Easy

### Code (All Methods)

**Method A — Greedy with Min Price Tracker**
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = float(inf)
        profit = 0
        for price in prices:
            if price < min_price:
                min_price = price
            elif price - min_price > profit:
                profit = price - min_price
        return profit
```

**Method B — Two Pointer (Buy/Sell Pointers)**
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        buy = 0
        sell = 1
        while sell < len(prices):
            if prices[buy] < prices[sell]:
                profit = max(profit, prices[sell] - prices[buy])
            else:
                buy = sell
            sell += 1
        return profit
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | Track `min_price` seen so far. At every step, check if current price gives a better profit. | O(n) | O(1) | ✅ Slightly cleaner |
| B | Two pointers `buy` and `sell`. If prices[buy] < prices[sell] → valid profit window. Else move buy to sell (new low found). | O(n) | O(1) | ✅ Very intuitive |

Both are equally optimal. The difference is **perspective**:
- Method A thinks: *"What is the minimum I could have bought at?"*
- Method B thinks: *"What is the best buy-sell pair I can form with two pointers?"*

**Method B insight:** When `prices[sell] <= prices[buy]`, the current `sell` index is a better (lower) buy point than the old `buy`. So we move `buy = sell`. This is why we never miss the optimal pair.

**`float('inf')`** in Method A: Python's way of representing positive infinity. `float(inf)` needs `from math import inf` OR you write `float('inf')` (string form). Any real price will be less than this, so min_price updates on the very first element.

---

### Learning / Takeaway

- This is a **greedy** problem — at each step, make the locally optimal choice.
- You only need **one pass** — no need to check every pair like brute force O(n²).
- **Key constraint**: You must buy before you sell. You cannot "go back in time."
- `max(profit, current_profit)` pattern is essential — always compare against the best seen so far.
- `float('inf')` is useful to initialize a "minimum tracker" so the first comparison always updates it.

---

### Pattern to Remember

```
PATTERN: "Find max profit with one buy and one sell"
→ Greedy: track min_price seen so far, update max profit at each step

TEMPLATE:
    min_price = float('inf')
    profit = 0
    for price in prices:
        min_price = min(min_price, price)
        profit = max(profit, price - min_price)
    return profit

TWO POINTER VERSION:
    buy, sell = 0, 1
    while sell < len(prices):
        if prices[buy] < prices[sell]:
            profit = max(profit, prices[sell] - prices[buy])
        else:
            buy = sell      ← new lowest found, shift buy
        sell += 1
```

---
---

## 2. Running Sum of 1d Array — LC #1480 | Easy

### Code (All Methods)

**Method A — Manual Prefix Sum with Temp Variable**
```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        output = []
        temp = nums[0]
        output.append(temp)
        for i in range(1, len(nums)):
            temp += nums[i]
            output.append(temp)
        return output
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | Keep a running `temp` variable. Add current element, append to output at each step. | O(n) | O(n) | ✅ Correct and clear |

This is the direct definition of a **prefix sum array**:
- `output[0] = nums[0]`
- `output[i] = output[i-1] + nums[i]`

The `temp` variable is just carrying forward `output[i-1]` without needing to index into output again.

**Alternative one-liner** (good to know for Python):
```python
import itertools
return list(itertools.accumulate(nums))
```
`itertools.accumulate` does exactly this — builds a running cumulative sum.

---

### Learning / Takeaway

- This is the **foundational prefix sum** problem — understand it deeply because every harder problem (LC #724, #238, #560, #303) builds on this idea.
- **Prefix sum definition**: `prefix[i] = nums[0] + nums[1] + ... + nums[i]`
- Once you have a prefix array, you can find the **sum of any subarray** in O(1):
  - `sum(i to j) = prefix[j] - prefix[i-1]`   (if i > 0)
  - `sum(0 to j) = prefix[j]`
- This O(1) range query is the entire reason prefix sums exist.

---

### Pattern to Remember

```
PATTERN: "Build cumulative sum array"
→ prefix[i] = prefix[i-1] + nums[i]

TEMPLATE:
    prefix = [0] * len(nums)
    prefix[0] = nums[0]
    for i in range(1, len(nums)):
        prefix[i] = prefix[i-1] + nums[i]

RANGE SUM QUERY using prefix:
    sum(i, j) = prefix[j] - (prefix[i-1] if i > 0 else 0)

Python shortcut: list(itertools.accumulate(nums))
```

---
---

## 3. Find Pivot Index — LC #724 | Easy

### Code (All Methods)

**Method A — Prefix Sum with Total (Optimal)**
```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        total = sum(nums)
        left_sum = 0
        for i, val in enumerate(nums):
            if left_sum == total - val - left_sum:
                return i
            left_sum += val
        return -1
```

**Method B — Brute Force (Recompute Left/Right at Each Index)**
```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        i = 0
        while i < len(nums):
            left_sum = 0
            right_sum = 0
            for j in range(0, i):
                left_sum += nums[j]
            for k in range(i+1, len(nums)):
                right_sum += nums[k]
            if left_sum == right_sum:
                return i
                break
            else: i += 1
        return -1
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | Compute total once. At each index, check if `left_sum == right_sum` using the formula. Move left_sum forward. | O(n) | O(1) | ✅ **Best** |
| B | For every index i, loop left to compute left_sum, loop right to compute right_sum. | O(n²) | O(1) | ❌ Inefficient |

**The key formula in Method A:**
```
right_sum = total - val - left_sum
pivot condition: left_sum == right_sum
→ left_sum == total - val - left_sum
→ 2 * left_sum + val == total
```
This is the elegant insight — you don't need to compute right_sum separately. It's just `total - current_element - left_sum`.

**Method B bug note:** The `break` after `return i` is dead code — `return` exits the function immediately, so `break` never executes.

---

### Learning / Takeaway

- `total = sum(nums)` is O(n), one time. Then the loop is O(n). Overall O(n).
- The derivation `right_sum = total - val - left_sum` is the core trick — **memorize it**.
- Notice `left_sum` is updated **after** the pivot check — you want the sum of elements strictly to the **left** of `i`, not including `i`.
- `enumerate(nums)` gives `(index, value)` — more Pythonic than `range(len(nums))` when you need both.

---

### Pattern to Remember

```
PATTERN: "Find index where left sum == right sum"
→ Use: total = sum(nums), track left_sum as you go

FORMULA:
    right_sum = total - current_val - left_sum
    pivot condition: left_sum == right_sum
    i.e., left_sum == total - val - left_sum

TEMPLATE:
    total = sum(nums)
    left_sum = 0
    for i, val in enumerate(nums):
        if left_sum == total - val - left_sum:
            return i
        left_sum += val
    return -1

KEY: Update left_sum AFTER the check (not before)
```

---
---

## 4. Product of Array Except Self — LC #238 | Medium

### Code (All Methods)

**Method A — Brute Force (Recompute Prefix & Suffix at Each Index)**
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        output = []
        i = 0
        while i < len(nums):
            prefix = 1
            suffix = 1
            for j in range(0, i):
                prefix *= nums[j]
            for k in range(i+1, len(nums)):
                suffix *= nums[k]
            output.append(prefix * suffix)
            i += 1
        return output
```

**Method B — Precomputed Prefix & Suffix Arrays**
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        output = [1] * len(nums)
        prefix = [1] * len(nums)
        suffix = [1] * len(nums)

        for i in range(1, len(nums)):
            prefix[i] = prefix[i-1] * nums[i-1]
        for i in range(len(nums)-2, -1, -1):
            suffix[i] = suffix[i+1] * nums[i+1]
        for i in range(len(nums)):
            output[i] = prefix[i] * suffix[i]
        return output
```

**Method C — Optimized (No Extra Arrays, O(1) Space)**
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        output = [1] * len(nums)
        
        prefix = 1
        for i in range(len(nums)):
            output[i] = prefix
            prefix *= nums[i]
        
        suffix = 1
        for i in range(len(nums)-1, -1, -1):
            output[i] *= suffix
            suffix *= nums[i]
        return output
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | For each index, recompute left product and right product by looping | O(n²) | O(1) extra | ❌ Too slow |
| B | Build full prefix[] and suffix[] arrays first, then multiply | O(n) | O(n) extra | ✅ Good |
| C | Use output[] itself to carry prefix, then multiply suffix in reverse pass | O(n) | O(1) extra | ✅✅ **Best** |

**How Method B's prefix/suffix arrays work:**
```
nums     = [1,  2,  3,  4]
prefix   = [1,  1,  2,  6]   → prefix[i] = product of all elements LEFT of i
suffix   = [24, 12, 4,  1]   → suffix[i] = product of all elements RIGHT of i
output   = [24, 12, 8,  6]   → output[i] = prefix[i] * suffix[i]
```
Notice:
- `prefix[0] = 1` (nothing to the left)
- `suffix[n-1] = 1` (nothing to the right)

**How Method C improves on B:**
- First pass (left to right): store running `prefix` product INTO `output[i]` — this fills output with prefix values.
- Second pass (right to left): multiply `suffix` INTO `output[i]` — this combines prefix (already in output) with suffix.
- No separate prefix[] or suffix[] arrays needed.

---

### Learning / Takeaway

- **Key constraint**: No division allowed. This rules out the naive approach of `total_product / nums[i]`.
- For any index `i`: `output[i] = (product of all elements left of i) × (product of all elements right of i)`
- This is the **prefix-product × suffix-product** pattern — the product equivalent of prefix sums.
- Method C's trick: reuse the output array as storage for prefix, then fold in suffix in a second pass. This is a classic space optimization technique.
- The reverse loop `range(len(nums)-1, -1, -1)` — memorize this. It means: start from last index, go to 0 (inclusive), step -1.

---

### Pattern to Remember

```
PATTERN: "Product of all elements except self, no division"
→ output[i] = prefix_product[i] × suffix_product[i]

OPTIMAL TEMPLATE (O(n) time, O(1) space):
    output = [1] * n

    # Pass 1: fill output with prefix products
    prefix = 1
    for i in range(n):
        output[i] = prefix
        prefix *= nums[i]

    # Pass 2: multiply suffix products into output
    suffix = 1
    for i in range(n-1, -1, -1):
        output[i] *= suffix
        suffix *= nums[i]

    return output

REVERSE LOOP: range(len(nums)-1, -1, -1)  ← go from last index to 0

EXAMPLE:
    nums   = [1, 2, 3, 4]
    After Pass 1: output = [1, 1, 2, 6]   (prefix products)
    After Pass 2: output = [24, 12, 8, 6]  (× suffix products)
```

---
---

## 5. Subarray Sum Equals K — LC #560 | Medium

### Code (All Methods)

**Method A — Brute Force (All Subarrays)**
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        count = 0
        for i in range(len(nums)):
            sum = 0
            for j in range(i, len(nums)):
                sum += nums[j]
                if sum == k:
                    count += 1
        return count
```

**Method B — Prefix Sum + HashMap (Optimal)**
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        output = 0
        temp = 0
        prefix = {0: 1}
        for n in nums:
            temp += n
            diff = temp - k
            output += prefix.get(diff, 0)
            prefix[temp] = 1 + prefix.get(temp, 0)
        return output
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | Try all starting points i, extend subarray to j, accumulate sum | O(n²) | O(1) | ❌ Too slow for large inputs |
| B | Track running prefix sum. Use a HashMap to count how many previous prefix sums satisfy `prefix[j] - prefix[i] = k` | O(n) | O(n) | ✅✅ **Best** |

**The math behind Method B — this is the most important part:**

If `prefix[j]` is the cumulative sum up to index `j`, and we want a subarray from `i+1` to `j` that sums to `k`:
```
prefix[j] - prefix[i] = k
→ prefix[i] = prefix[j] - k
→ diff = temp - k
```
So at each step, we ask: *"How many times has the value `temp - k` appeared as a prefix sum before?"* Each occurrence represents a valid subarray ending at the current index.

**Why `prefix = {0: 1}`?**
The `0: 1` initialization handles the case where the subarray starts from index 0. If `temp == k`, then `diff = 0`, and we need `prefix[0]` to exist with count 1.

**Why `prefix.get(diff, 0)` and not `prefix[diff]`?**
`prefix[diff]` would throw a `KeyError` if `diff` was never seen. `.get(diff, 0)` safely returns 0 if not found.

---

### Learning / Takeaway

- Brute force works here but fails at scale. Always look for the prefix-sum-hashmap pattern when the problem says **count subarrays with sum = k**.
- **Never use `sum` as a variable name** — it shadows Python's built-in `sum()` function. Use `curr_sum` or `temp` instead (your Method B correctly uses `temp`).
- The formula `diff = temp - k` → *"look up how many times this value appeared before"* is the core trick.
- Initializing `{0: 1}` is a **very common prefix sum trick** — it handles subarrays that start from index 0.
- This pattern works even with **negative numbers** (unlike sliding window which requires all positives).

---

### Pattern to Remember

```
PATTERN: "Count subarrays with sum equal to k"
→ Prefix Sum + HashMap

TEMPLATE:
    prefix = {0: 1}    ← crucial initialization
    curr_sum = 0
    count = 0
    for num in nums:
        curr_sum += num
        diff = curr_sum - k
        count += prefix.get(diff, 0)
        prefix[curr_sum] = prefix.get(curr_sum, 0) + 1
    return count

KEY FORMULA: curr_sum - k = prefix sum we're looking for
WHY {0:1}: handles subarrays starting from index 0
NOTE: Works with negative numbers (unlike sliding window)
```

---
---

## 6. Range Sum Query — Immutable — LC #303 | Easy

### Code (All Methods)

**Method A — Prefix Sum in Constructor, O(1) Query**
```python
class NumArray(object):

    def __init__(self, nums):
        self.prefix = []
        temp = 0
        for n in nums:
            temp += n
            self.prefix.append(temp)

    def sumRange(self, left, right):
        right_sum = self.prefix[right]
        if left != 0:
            left_sum = self.prefix[left - 1]
        else:
            left_sum = 0
        return right_sum - left_sum
```

---

### Logic of Each Method

| Method | Approach | Time (Build) | Time (Query) | Space | Better? |
|--------|----------|-------------|-------------|-------|---------|
| A | Build prefix array in `__init__`. Answer any range query in O(1) using `prefix[right] - prefix[left-1]` | O(n) | O(1) | O(n) | ✅ **Best** |

**How the query works:**
```
prefix = [cumulative sums]
sumRange(left, right) = prefix[right] - prefix[left-1]

Example:
nums   = [-2, 0, 3, -5, 2, -1]
prefix = [-2, -2, 1, -4, -2, -3]

sumRange(0, 2) = prefix[2] - 0          = 1        ✓  (-2+0+3=1)
sumRange(2, 5) = prefix[5] - prefix[1]  = -3-(-2)  = -1  ✓
sumRange(0, 5) = prefix[5] - 0          = -3       ✓
```

**The `if left != 0` guard**: When left is 0, there's no `prefix[-1]` to look up. `prefix[-1]` in Python is the **last element**, not an error — which would give a wrong answer silently. Always guard this case.

**Design pattern:** This is an **OOP-based problem** — `__init__` does preprocessing once, and `sumRange` answers many queries fast. This is the classic **precomputation** pattern.

---

### Learning / Takeaway

- **When a function is called many times**, precompute in the constructor. Don't recompute on every call.
- `prefix[right] - prefix[left-1]` is the **fundamental prefix sum range query formula**. Memorize it.
- The edge case when `left == 0` means there's no element to the left — treat `prefix[left-1]` as 0.
- Python pitfall: `prefix[-1]` doesn't error — it gives the last element. This makes the `left==0` bug silent and dangerous.

---

### Pattern to Remember

```
PATTERN: "Answer range sum queries in O(1) after O(n) preprocessing"
→ Build prefix array once, query with formula

RANGE QUERY FORMULA:
    sumRange(left, right) = prefix[right] - (prefix[left-1] if left > 0 else 0)

TEMPLATE:
    # Build (in __init__):
    prefix = []
    curr = 0
    for n in nums:
        curr += n
        prefix.append(curr)

    # Query:
    def sumRange(left, right):
        right_val = prefix[right]
        left_val  = prefix[left-1] if left > 0 else 0
        return right_val - left_val

COMMON BUG: prefix[-1] in Python = last element, NOT an error.
Always guard the left==0 case explicitly.
```

---
---

## 7. Corporate Flight Bookings — LC #1109 | Medium

### Code (All Methods)

**Method A — Brute Force (Direct Increment for Each Booking Range)**
```python
class Solution:
    def corpFlightBookings(self, bookings: List[List[int]], n: int) -> List[int]:
        labels = []
        temp = [0] * n
        for i in range(1, n+1):
            labels.append(i)
        map = dict(zip(labels, temp))
        for entry in bookings:
            x = entry[0]
            y = entry[1]
            for j in range(x, y+1):
                map[j] += entry[2]
        return list(map.values())
```

**Method B — Difference Array (Optimal)**
```python
class Solution:
    def corpFlightBookings(self, bookings: List[List[int]], n: int) -> List[int]:
        answer = [0] * (n + 1)

        for first, last, seats in bookings:
            answer[first - 1] += seats
            if last < n:
                answer[last] -= seats
        
        for i in range(1, n):
            answer[i] += answer[i - 1]
        
        return answer[:n]
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | For each booking, iterate through all flights in range [first, last] and increment. | O(n × m) where m = bookings | O(n) | ❌ Slow for large ranges |
| B | Difference array: mark +seats at start, -seats after end. One prefix sum pass recovers final values. | O(n + m) | O(n) | ✅✅ **Best** |

**The Difference Array concept — core of Method B:**

Instead of incrementing every index in a range, just mark the **boundaries**:
- `diff[first-1] += seats` → range starts here
- `diff[last] -= seats` → range ends (exclusive)

Then a single prefix sum pass "spreads" these boundary marks into actual values across the range.

```
Example: n=5, bookings = [[1,2,10],[2,3,20],[2,5,25]]
                  flights:  1   2   3   4   5
diff array:              [10, 20, 20, 0, 25, 0]  ← after marking boundaries
                         +10 +20 +20  0 +25    (first-1 additions)
                              -10 -20          (last subtractions)
After prefix pass:       [10, 30, 40, 25, 25]  ✓
```

**Method A uses a dict `map`** — this is unnecessary overhead. A plain array would be simpler and faster. Also, `map` is a Python built-in function — never use it as a variable name.

---

### Learning / Takeaway

- **Difference Array** is a powerful pattern for **range update queries**. Instead of updating O(range) elements, update just 2 boundary points.
- The technique: `diff[l] += val` and `diff[r+1] -= val`. Then prefix sum gives actual values.
- This is the **inverse of prefix sum** — prefix sum answers range queries, difference array answers range updates.
- `answer = [0] * (n + 1)` — the +1 extra size is to avoid index-out-of-bounds when `last == n` (the subtraction would land at index `n`, which is within bounds).
- `for first, last, seats in bookings` — Python tuple unpacking in a for loop. Much cleaner than `entry[0], entry[1], entry[2]`.

---

### Pattern to Remember

```
PATTERN: "Apply +value to a range [l, r] across many queries"
→ Difference Array

TEMPLATE:
    diff = [0] * (n + 1)    ← +1 to safely handle r = n

    # Mark boundaries (1-indexed input → convert to 0-indexed):
    for first, last, val in bookings:
        diff[first - 1] += val
        diff[last] -= val        ← last (not last+1) because input is 1-indexed

    # Recover actual values with prefix sum:
    for i in range(1, n):
        diff[i] += diff[i - 1]

    return diff[:n]

DIFFERENCE ARRAY vs PREFIX SUM:
    Prefix Sum    → precompute to answer range QUERIES fast
    Difference Array → precompute to answer range UPDATES fast
```

---
---

## 8. Car Pooling — LC #1094 | Medium

### Code (All Methods)

**Method A — Events / Sorting Approach**
```python
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        temp = []
        for ppl, start, end in trips:
            temp.append((start, +ppl))
            temp.append((end, -ppl))
        temp.sort()
        for _, ppl in temp:
            capacity -= ppl
            if capacity < 0:
                return False
        return True
```

**Method B — Difference Array on Fixed-Size Location Array (Optimal)**
```python
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        temp = [0] * 1001
        for ppl, start, end in trips:
            temp[start] += ppl
            temp[end] -= ppl
        
        curr_ppl = 0
        for change in temp:
            curr_ppl += change
            if curr_ppl > capacity:
                return False
        else: return True
```

---

### Logic of Each Method

| Method | Approach | Time | Space | Better? |
|--------|----------|------|-------|---------|
| A | Create events: `(location, +ppl)` for pickup, `(location, -ppl)` for drop. Sort by location. Simulate capacity. | O(n log n) | O(n) | ✅ Flexible, works for any location range |
| B | Difference array of fixed size 1001 (max location per constraints). Mark pickups and drops, then prefix sum and check. | O(n + 1001) ≈ O(n) | O(1001) ≈ O(1) | ✅✅ **Best for constrained locations** |

**How Method A (Events/Sweep Line) works:**
- Convert each trip into 2 events: `(start, +ppl)` and `(end, -ppl)`.
- Sort all events by location.
- Traverse in order, updating remaining capacity. If it goes negative, return False.

**Critical sorting note in Method A**: Python's `tuple sort` is lexicographic — it sorts by first element (location), then by second element (ppl change). At the same location, `-ppl` (negative) sorts before `+ppl` (positive). This means drop-offs happen before pick-ups at the same location — which is **correct behavior** (passengers get off first, then new ones board).

**Method B** treats `start` as pickup (+ppl) and `end` as drop-off (-ppl). Why `temp[end] -= ppl` and not `temp[end-1]`? Because passengers ride from `start` **up to but not including** `end` — they drop off **at** `end`, so the subtraction happens at `end`.

**`else` after `for` loop** (Method B): In Python, `for...else` — the `else` block runs if the loop completes without a `break`. Since there's no `break` here, `else: return True` always runs if we exit the loop normally. It's equivalent to `return True` after the loop — just unusual syntax.

---

### Learning / Takeaway

- This is the same **difference array** idea as LC #1109 — mark pickup and drop-off points, check if at any location the count exceeds capacity.
- Method A's **events + sort** approach is the generalized pattern (works even if location range is unknown or huge).
- Method B works because constraints say `0 <= from_i < to_i <= 1000` — max location is 1000, so an array of size 1001 covers all cases.
- **Sweep line algorithm** (Method A) is a broader concept: convert intervals into events, sort, process in order. Used in meeting rooms, interval scheduling, flight bookings, etc.
- Avoid `for...else` in production/interviews unless you're sure the reviewer knows it — prefer explicit `return True` after the loop.

---

### Pattern to Remember

```
PATTERN: "Check if capacity is exceeded at any point across multiple intervals"
→ Two approaches: Events+Sort OR Difference Array

METHOD 1 — Events / Sweep Line (general):
    events = []
    for ppl, start, end in trips:
        events.append((start, +ppl))
        events.append((end, -ppl))
    events.sort()
    curr = 0
    for _, delta in events:
        curr += delta
        if curr > capacity: return False
    return True

METHOD 2 — Difference Array (when location range is bounded):
    diff = [0] * (max_location + 1)
    for ppl, start, end in trips:
        diff[start] += ppl
        diff[end]   -= ppl        ← passengers leave AT end, not end-1
    curr = 0
    for change in diff:
        curr += change
        if curr > capacity: return False
    return True

PICKUP vs DROP: passengers leave AT end, so subtract at diff[end] not diff[end-1]
```

---
---

## Cheat Sheet — Everything to Remember for Arrays Topic

---

### Core Array Techniques — When to Use What

| Technique | Use When | Time | Space |
|-----------|----------|------|-------|
| **Prefix Sum** | Range sum queries, count subarrays with target sum | O(n) build, O(1) query | O(n) |
| **Suffix Product** | Product of elements to the right of index | O(n) | O(n) or O(1) |
| **Prefix + Suffix Product** | Product of array except self | O(n) | O(1) |
| **Difference Array** | Range update queries (add value to range [l,r]) | O(n + m) | O(n) |
| **Greedy + Running Min/Max** | Stock buy-sell, tracking best seen so far | O(n) | O(1) |
| **Prefix Sum + HashMap** | Count subarrays with exact sum = k | O(n) | O(n) |
| **Sweep Line / Events** | Intervals, overlapping ranges, capacity problems | O(n log n) | O(n) |

---

### Python Tricks Used in This Topic

```python
# float('inf') — positive infinity, any number is smaller
min_price = float('inf')

# float('-inf') — negative infinity, any number is larger
max_val = float('-inf')

# Initialize list with default value
arr = [0] * n
arr = [1] * n

# Tuple swap (Python only) — no temp variable needed
arr[i], arr[j] = arr[j], arr[i]

# Tuple unpacking in for loop
for first, last, seats in bookings:
    ...

# enumerate — get index AND value together
for i, val in enumerate(nums):
    ...

# range for reverse traversal
for i in range(len(nums)-1, -1, -1):    # from last index to 0
    ...

# dict.get(key, default) — safe lookup with fallback
prefix.get(diff, 0)     # returns 0 if diff not in prefix

# zip + dict — pair two lists into a dictionary
d = dict(zip(keys_list, values_list))

# Never shadow built-ins — AVOID these as variable names:
# sum, map, list, dict, set, min, max, input, id, type
```

---

### Prefix Sum Templates

```python
# 1. Build prefix array
prefix = [0] * n
prefix[0] = nums[0]
for i in range(1, n):
    prefix[i] = prefix[i-1] + nums[i]

# 2. Range sum query
def rangeSum(l, r):
    return prefix[r] - (prefix[l-1] if l > 0 else 0)

# 3. Prefix sum + HashMap (count subarrays with sum = k)
prefix_map = {0: 1}
curr_sum = 0
count = 0
for num in nums:
    curr_sum += num
    count += prefix_map.get(curr_sum - k, 0)
    prefix_map[curr_sum] = prefix_map.get(curr_sum, 0) + 1
```

---

### Difference Array Template

```python
# Apply +val to range [l, r] (0-indexed) across m operations, then recover
diff = [0] * (n + 1)
for l, r, val in operations:
    diff[l] += val
    diff[r + 1] -= val          # r+1 to not affect index r+1

# Recover actual values
for i in range(1, n):
    diff[i] += diff[i-1]

result = diff[:n]

# NOTE: When input is 1-indexed (like flight numbers):
diff[first - 1] += val
diff[last] -= val               # last (not last+1) because of 1-indexed to 0-indexed conversion
```

---

### Key Formulas to Memorize

```
Prefix Sum:
    prefix[i] = prefix[i-1] + nums[i]
    rangeSum(l, r) = prefix[r] - prefix[l-1]     (guard l==0)

Prefix Product (left of i):
    prefix[0] = 1
    prefix[i] = prefix[i-1] * nums[i-1]

Suffix Product (right of i):
    suffix[n-1] = 1
    suffix[i] = suffix[i+1] * nums[i+1]

Product Except Self:
    output[i] = prefix[i] * suffix[i]

Pivot Index:
    left_sum == total - nums[i] - left_sum
    equivalently: 2 * left_sum + nums[i] == total

Subarray Sum = k:
    curr_sum - prefix[i] = k  →  look up (curr_sum - k) in prefix map

Difference Array Range Update:
    diff[l] += val  ;  diff[r+1] -= val
    then prefix sum to recover
```

---

### Common Mistakes to Avoid

| Mistake | Why it's Wrong | Fix |
|---------|---------------|-----|
| Using `prefix[-1]` when `left == 0` | Python's -1 index = last element, not an error — gives wrong answer | Always guard: `if left > 0 else 0` |
| Using `sum` as variable name | Shadows Python's built-in `sum()` function | Use `curr_sum`, `total`, `temp` |
| Using `map` as variable name | Shadows Python's built-in `map()` function | Use `freq`, `counts`, `d` |
| Forgetting `{0: 1}` in prefix-sum hashmap | Misses subarrays starting from index 0 | Always initialize `prefix_map = {0: 1}` |
| Updating `left_sum` before pivot check | Includes current element in left_sum incorrectly | Update `left_sum` AFTER the check |
| `break` after `return` | Dead code — `return` exits function immediately | Remove the `break` |
| Sorting by events without handling ties | At same location, drop-off should happen before pick-up | Python tuple sort handles this naturally: (-ppl sorts before +ppl) |

---

*Notes compiled from LeetCode practice — Topic 2: Arrays (Prefix, Suffix, Difference Array)*
*Problems: LC #121, #1480, #724, #238, #560, #303, #1109, #1094*
