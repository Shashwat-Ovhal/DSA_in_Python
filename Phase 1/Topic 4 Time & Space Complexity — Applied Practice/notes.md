### Time & Space Complexity — Applied Practice

***

## 1. Two Sum — LC #1

### Code (All Methods)

**Method A — Brute Force (Nested Loops)**

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i]+nums[j] == target:
                    output = (i, j)
        return output
```

**Method B — Dictionary (Value → Key Search, Inefficient)**

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = dict(enumerate(nums))
        for i in range (len(nums)):
            match = target - nums[i]
            for key, val in d.items():
                if val == match and key != i:
                    return [i, key]
```

**Method C — Dictionary (Complement Lookup, Optimal)**

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {val:i for i, val in enumerate(nums)}
        for i, val in enumerate(nums):
            match = target - val
            if match in d and d[match] != i:
                return [i, d[match]]
```

***

### Logic of Each Method

| Method | Approach                                                                                             | Time  | Space |
| ------ | ---------------------------------------------------------------------------------------------------- | ----- | ----- |
| A      | Try every pair (i, j). If sum == target, store result.                                               | O(n²) | O(1)  |
| B      | Build dict of index→value. For each element, loop entire dict to find complement.                    | O(n²) | O(n)  |
| C      | Build dict of value→index. For each element, check if its complement already exists in dict in O(1). | O(n)  | O(n)  |

* **Method A** is pure brute force. Note: it doesn't `return` early — it overwrites `output` until the last matching pair. Bug-prone.

* **Method B** uses a dictionary but still has a nested loop (looping `.items()` is O(n)), so no improvement over A.

* **Method C** is the true optimal. The key insight: instead of finding a pair, you find one number and look for its "partner" instantly using a hash map.

***

### Learning / Takeaway

* When you need to find **two elements that satisfy a condition**, think: *can I store one element and look up the other in O(1)?*

* **`dict(enumerate(nums))`** → creates `{0: val0, 1: val1, ...}` (index → value).

* **`{val: i for i, val in enumerate(nums)}`** → creates `{value: index}` (value → index). This is what makes lookup fast.

* The condition `d[match] != i` prevents using the **same element twice**.

* Method A has a subtle bug — it doesn't return immediately, so if multiple pairs exist it gives the last one. Always `return` early when the answer is found.

***

### Pattern to Remember

```
PATTERN: "Find two numbers that sum to target"
→ Use a HashMap {value: index}
→ For each num, compute complement = target - num
→ If complement is in map AND its index != current index → answer found
→ Time: O(n) | Space: O(n)

KEY FORMULA: complement = target - current_element
```

***

***

## 2. Contains Duplicate — LC #217

### Code (All Methods)

**Method A — Dictionary (Store Last Index)**

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        d = {num:i for i, num in enumerate(nums)}
        for i in range(len(nums)):
            if nums[i] in d and d[nums[i]]!=i:
                return True
                
        return False
```

**Method B — Set Length Comparison**

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        s = set(nums)
        if len(s)<len(nums):
            return True
        else:
            return False
```

**Method C — Set with Early Return (Optimal)**

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        s = set()
        for n in nums:
            if n in s:
                return True
            else: s.add(n)
        return False
```

***

### Logic of Each Method

| Method | Approach                                                                                             | Time           | Space |
| ------ | ---------------------------------------------------------------------------------------------------- | -------------- | ----- |
| A      | Build dict {value: last\_index}. If a value maps to a different index than current, duplicate found. | O(n)           | O(n)  |
| B      | Convert entire list to set (removes duplicates). If set is smaller, duplicate existed.               | O(n)           | O(n)  |
| C      | Traverse list, add to set one-by-one. If element already in set, return True immediately.            | O(n) best case | O(n)  |

* **Method A** has a **bug**: `{num: i for i, num in enumerate(nums)}` overwrites the index with the **last occurrence**. So when you check `d[nums[i]] != i`, it works for earlier occurrences but the logic is fragile and misleading.

* **Method B** is clean and elegant. Works because sets don't allow duplicates — any shrinkage = duplicate.

* **Method C** is the most **interview-friendly** because it returns early the moment it finds a duplicate. Best-case O(1), worst-case O(n).

***

### Learning / Takeaway

* **Set** is the go-to structure when you only care about *existence* (seen or not seen), not the value or index.

* `set(list)` removes duplicates — comparing `len(set) < len(list)` is a clever one-liner check.

* **Early return** is always preferred in interviews — it shows you think about best-case performance.

* Dictionary approach (Method A) is overly complex for this problem — don't overcomplicate things.

***

### Pattern to Remember

```
PATTERN: "Check if any element has been seen before"
→ Use a Set (seen = set())
→ Traverse array:
     if element in seen → duplicate found, return True
     else → seen.add(element)
→ Time: O(n) | Space: O(n)

SHORTCUT CHECK: if len(set(nums)) < len(nums) → duplicate exists
```

***

***

## 3. Majority Element — LC #169

### Code (All Methods)

**Method A — Counter Most Common**

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        counts = Counter(nums)
        return counts.most_common(1)[0][0]
```

**Method B — Manual Count with Early Return (Optimal without library)**

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        counts = {}
        threshold = len(nums)//2
        for num in nums:
            if num in counts:
                counts[num] += 1
            else: counts[num]=1
            if counts[num] > threshold:
                return num
```

***

### Logic of Each Method

| Method | Approach                                                                                           | Time           | Space |
| ------ | -------------------------------------------------------------------------------------------------- | -------------- | ----- |
| A      | Use Python's `Counter` to count all frequencies. `most_common(1)` returns the top element.         | O(n)           | O(n)  |
| B      | Manually build frequency dict. As soon as any count exceeds n//2, return that element immediately. | O(n) best case | O(n)  |

* **Method A** is clean but relies on the `Counter` library. It always processes the whole array.

* **Method B** builds the count manually and returns as soon as the threshold is crossed — smarter for large arrays where the majority element appears early.

* Note: For this problem, the **Boyer-Moore Voting Algorithm** exists that solves it in O(n) time and O(1) space — worth knowing.

***

### Learning / Takeaway

* **`Counter(nums)`** from `collections` builds `{element: count}` automatically.

* **`most_common(1)`** returns a list of tuples: `[(element, count)]`, so `[0][0]` gives the element.

* The **threshold** for majority element is always `n // 2` (more than half).

* Whenever you need frequency counts, `Counter` or a manual dict both work. Manual dict gives you more control (like early return).

* **Boyer-Moore** (advanced): Keep a `candidate` and `count`. Increment if same, decrement if different. Reset when count hits 0. Final candidate is always the majority.

***

### Pattern to Remember

```
PATTERN: "Find element appearing more than n/2 times"
→ Frequency HashMap approach:
     threshold = len(nums) // 2
     counts = {}
     for each num: increment count, if count > threshold → return
→ Time: O(n) | Space: O(n)

ADVANCED PATTERN (Boyer-Moore): O(n) Time, O(1) Space
     candidate, count = nums[0], 0
     for num in nums:
         if count == 0: candidate = num
         count += (1 if num == candidate else -1)
     return candidate
```

***

***

## 4. Sort Colors — LC #75

### Code (All Methods)

**Method A — Bubble Sort (Manual Swap with Temp)**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i]>nums[j]:
                    temp = nums[i]
                    nums[i] = nums[j]
                    nums[j] = temp
```

**Method B — Bubble Sort (Python Tuple Swap)**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i]>nums[j]:
                    nums[i], nums[j] = nums[j], nums[i]
```

***

### Logic of Each Method

| Method | Approach                                            | Time  | Space |
| ------ | --------------------------------------------------- | ----- | ----- |
| A      | Selection/Bubble sort with temp variable for swap   | O(n²) | O(1)  |
| B      | Same as A but uses Python's simultaneous tuple swap | O(n²) | O(1)  |

* Both methods are functionally identical — same O(n²) time, O(1) space.

* The only difference is **how the swap is done**:

  * Method A: uses a `temp` variable (the "classic" 3-step swap — safe to explain in any language)

  * Method B: uses Python's `a, b = b, a` which does the same thing in one line

* The **optimal solution** for this specific problem is the **Dutch National Flag Algorithm** (DNF) by Edsger Dijkstra — O(n) time, O(1) space using three pointers: `low`, `mid`, `high`.

***

### Learning / Takeaway

* **In-place** means you must modify the original array, not return a new one. Space must be O(1).

* **Temp swap** (3 lines) vs **tuple swap** (1 line in Python): both are O(1). Know both.

* This problem has values only 0, 1, 2 — the constraints matter! Always read them.

* **Dutch National Flag** is the intended pattern here:

  * 0s go to the front (left of `low`)

  * 2s go to the back (right of `high`)

  * 1s stay in the middle

***

### Pattern to Remember

```
PATTERN: "Sort array with only 3 distinct values in O(n)"
→ Dutch National Flag Algorithm (3 Pointers):
     low = 0, mid = 0, high = len(nums) - 1
     while mid <= high:
         if nums[mid] == 0: swap(nums[low], nums[mid]); low++; mid++
         elif nums[mid] == 1: mid++
         else: swap(nums[mid], nums[high]); high--
→ Time: O(n) | Space: O(1)

SWAP PATTERNS:
     Classic:  temp = a; a = b; b = temp
     Python:   a, b = b, a        ← same result, 1 line
```

***

***

## 5. Find the Duplicate Number — LC #287

### Code (All Methods)

**Method A — Counter**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        count = Counter(nums)
        return count.most_common(1)[0][0]
```

**Method B — Sort then Check Adjacent**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        nums.sort()
        for i in range(0, len(nums)-1):
            if nums[i] == nums[i+1]:
                return nums[i]
```

**Method C — Set (Seen Before)**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        s = set()
        for num in nums:
            if num in s:
                return num
            else: 
                s.add(num)
```

**Method D — Floyd's Cycle Detection (Starting from nums\[0])**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        slow=fast=nums[0]
        slow=nums[slow]
        fast=nums[nums[fast]]
        while(slow!=fast):
            slow=nums[slow]
            fast=nums[nums[fast]]

        fast=nums[0]
        while slow!=fast:
            slow=nums[slow]
            fast=nums[fast]
        return slow
```

**Method E — Floyd's Cycle Detection (Starting from index 0)**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        slow = 0
        fast = 0
        slow=nums[slow]
        fast=nums[nums[fast]]
        while(slow!=fast):
            slow=nums[slow]
            fast=nums[nums[fast]]
        
        slow=0
        while slow!=fast:
            slow=nums[slow]
            fast=nums[fast]
        return slow
```

***

### Logic of Each Method

| Method | Approach                                                                                                                        | Time       | Space |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- | ---------- | ----- |
| A      | Counter builds frequency map, most\_common returns the duplicate                                                                | O(n)       | O(n)  |
| B      | Sort first, then check adjacent elements — equal neighbors = duplicate                                                          | O(n log n) | O(1)  |
| C      | Set-based seen-before tracking, return on first duplicate seen                                                                  | O(n)       | O(n)  |
| D      | Treat array as a linked list (value = next node). Floyd's algorithm finds cycle entry = duplicate. Starts pointers at `nums[0]` | O(n)       | O(1)  |
| E      | Same as D but pointers start at index `0` — the "dummy head" version                                                            | O(n)       | O(1)  |

**Floyd's Cycle — How it works:**

* Treat array as: index → value = next index (like a linked list)

* Since a number is duplicated, two indices point to the same next index → **cycle forms**

* **Phase 1**: Move `slow` by 1 step, `fast` by 2 steps until they meet (inside the cycle)

* **Phase 2**: Reset one pointer to start. Move both by 1 step. Where they meet = cycle entry = **duplicate number**

**Difference between D and E:**

* Method D: `slow = fast = nums[0]` — both start at the value at index 0, then take their first step before the loop

* Method E: `slow = fast = 0` — both start at index 0, then take their first step before the loop

* Both are valid. Method E is cleaner (starting from the "dummy" node 0, like a standard linked list cycle problem)

***

### Learning / Takeaway

* **Method B** (sort + adjacent check) is a useful fallback pattern whenever you need to find duplicates and can afford to modify the array.

* **Method C** (set) is the easiest O(n) approach to code in an interview.

* **Floyd's Cycle Detection** is the most impressive — O(n) time, O(1) space. It's the algorithm behind the classic **Linked List Cycle II** problem (LC #142).

* The constraint `nums[i]` is between 1 and n and array has n+1 elements — this is what guarantees a duplicate and makes Floyd's possible.

* **The array-as-linked-list trick**: treat the value at each index as a "pointer" to the next node. This is the key mental model for Floyd's here.

***

### Pattern to Remember

```
PATTERN: "Find duplicate in array of size n+1 where values are 1 to n"
→ This guarantees exactly one duplicate (Pigeonhole Principle)
→ Best approach: Floyd's Cycle Detection (O(n) time, O(1) space)

FLOYD'S TWO-PHASE TEMPLATE:
     Phase 1 — Find meeting point inside cycle:
         slow = nums[0]; fast = nums[nums[0]]  (OR slow=fast=0 version)
         while slow != fast:
             slow = nums[slow]
             fast = nums[nums[fast]]

     Phase 2 — Find cycle entry (= duplicate):
         fast = 0  (OR fast = nums[0] depending on start)
         while slow != fast:
             slow = nums[slow]
             fast = nums[fast]
         return slow

SIMPLER ALTERNATIVES:
     Set approach:  O(n) time, O(n) space
     Sort approach: O(n log n) time, O(1) space
```

***

***

## Python Dict & Set — Cheat Sheet for DSA Patterns

### Dictionary Methods

| Method / Syntax                     | What it Does                                  | Example                           |
| ----------------------------------- | --------------------------------------------- | --------------------------------- |
| `d = {}`                            | Create empty dict                             | `d = {}`                          |
| `d[key] = val`                      | Set a key-value pair                          | `d['a'] = 1`                      |
| `d[key]`                            | Get value (KeyError if missing)               | `d['a']` → `1`                    |
| `d.get(key, default)`               | Get value safely, return default if not found | `d.get('z', 0)` → `0`             |
| `key in d`                          | Check if key exists                           | `'a' in d` → `True`               |
| `d.keys()`                          | All keys                                      | `['a', 'b']`                      |
| `d.values()`                        | All values                                    | `[1, 2]`                          |
| `d.items()`                         | All key-value pairs                           | `[('a',1), ('b',2)]`              |
| `d.pop(key)`                        | Remove and return value                       | `d.pop('a')` → `1`                |
| `d.update({k:v})`                   | Merge another dict in                         | `d.update({'c': 3})`              |
| `dict(enumerate(lst))`              | Create `{index: value}` dict                  | `{0: 'a', 1: 'b'}`                |
| `{v: i for i, v in enumerate(lst)}` | Create `{value: index}` dict                  | `{'a': 0, 'b': 1}`                |
| `Counter(lst)`                      | Frequency count `{element: count}`            | `Counter([1,1,2])` → `{1:2, 2:1}` |
| `Counter.most_common(k)`            | Top k frequent elements as `[(elem, count)]`  | `c.most_common(1)` → `[(1, 2)]`   |
| `d[key] = d.get(key, 0) + 1`        | Safe frequency increment                      | Common pattern for counting       |

***

### Set Methods

| Method / Syntax | What it Does                               | Example                      |
| --------------- | ------------------------------------------ | ---------------------------- |
| `s = set()`     | Create empty set                           | `s = set()`                  |
| `s = set(lst)`  | Create set from list (removes duplicates)  | `set([1,1,2])` → `{1, 2}`    |
| `s.add(x)`      | Add element                                | `s.add(3)`                   |
| `s.remove(x)`   | Remove element (KeyError if not found)     | `s.remove(3)`                |
| `s.discard(x)`  | Remove element (no error if missing)       | `s.discard(99)`              |
| `x in s`        | Check membership — O(1)                    | `3 in s` → `True`            |
| `len(s)`        | Number of elements                         | `len(s)`                     |
| `s1 & s2`       | Intersection (common elements)             | `{1,2} & {2,3}` → `{2}`      |
| `s1 \| s2`      | Union (all elements)                       | `{1,2} \| {2,3}` → `{1,2,3}` |
| `s1 - s2`       | Difference (in s1 but not s2)              | `{1,2} - {2,3}` → `{1}`      |
| `s1 ^ s2`       | Symmetric difference (in one but not both) | `{1,2} ^ {2,3}` → `{1,3}`    |

***

### When to Use Dict vs Set

| Situation                                        | Use                      |
| ------------------------------------------------ | ------------------------ |
| Need to store value/index alongside element      | `dict`                   |
| Need to count frequency of elements              | `Counter` (dict)         |
| Only need to check "have I seen this before?"    | `set`                    |
| Need to check "does this complement/pair exist?" | `dict`                   |
| Need to remove duplicates from a list            | `set(list)`              |
| Need the most frequent element                   | `Counter.most_common(1)` |

***

### Common DSA Frequency Count Template

```python
# Manual dict (verbose, interview-safe)
counts = {}
for num in nums:
    if num in counts:
        counts[num] = counts[num] + 1
    else:
        counts[num] = 1

# OR using .get()
counts = {}
for num in nums:
    counts[num] = counts.get(num, 0) + 1

# OR using Counter (clean)
from collections import Counter
counts = Counter(nums)
```

***

### Common "Seen Before" Set Template

```python
seen = set()
for num in nums:
    if num in seen:
        # duplicate found / condition met
        return num
    else:
        seen.add(num)
```

***

*Notes compiled from LeetCode practice — Week 1 of 70-Day DSA Roadmap*
*Problems: LC #1, #217, #169, #75, #287*
