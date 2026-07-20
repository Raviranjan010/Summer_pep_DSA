# 🔍 04 — Binary Search

> **Explain Like I'm 5:** Finding a name in a physical phonebook. You don't start at page 1. You flip open the exact **middle**. If the name is alphabetically later, you throw away the entire left half of the book. You repeat this in the remaining pages. In just a few flips, you find the name out of millions!
>
> ⚠️ **The Golden Rule:** The input array **must be sorted**. If it is unsorted, binary search will fail.

---

## 📐 Why is it so fast? (The Mathematical Proof)

If you use Linear Search, looking for a target in an array of size $n$ takes up to $n$ steps.
In Binary Search, the search space is cut in half at every step. Let's see how many elements are left after $k$ steps:

- **Start:** $n$ elements
- **Step 1:** $\frac{n}{2}$ elements
- **Step 2:** $\frac{n}{4} = \frac{n}{2^2}$ elements
- **Step 3:** $\frac{n}{8} = \frac{n}{2^3}$ elements
- ...
- **Step $k$:** $\frac{n}{2^k}$ elements

The search terminates when the search space shrinks to $1$ element:

$$\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2(n)$$

Thus, the maximum number of steps is $\log_2(n)$.
- For $n = 1,000,000$ elements, linear search takes up to $1,000,000$ steps. Binary search takes at most $\mathbf{20}$ steps! ($\log_2(1,000,000) \approx 19.93$)

---

## 🎨 Visualizing Search Range Shrinkage

Here is a visual trace of searching for target `7` in the sorted array `[1, 3, 5, 7, 9, 11]`:

```
Step 1:
[ 1,  3,  5,  7,  9,  11 ]
  ▲           ▲        ▲
 low         mid      high
 arr[mid] = 5 < 7. Target is on the RIGHT. 
 Throw away left side. Set low = mid + 1 (index 3).

Step 2:
               [ 7,  9,  11 ]
                 ▲   ▲    ▲
                low mid  high
 arr[mid] = 9 > 7. Target is on the LEFT.
 Throw away right side. Set high = mid - 1 (index 3).

Step 3:
               [ 7 ]
                 ▲
             low, mid, high
 arr[mid] = 7 == 7. Found! Return index 3.
```

---

## 📋 The 3 Binary Search Templates

Not all Binary Search problems are about finding an exact number. There are 3 core patterns:

### Template 1: Pure Exact Match Search
Used when searching for a specific target in a sorted collection.

```python
# Template 1: Exact Match
low, high = 0, len(arr) - 1
while low <= high:
    mid = low + (high - low) // 2
    if arr[mid] == target:
        return mid  # Found it!
    elif arr[mid] < target:
        low = mid + 1
    else:
        high = mid - 1
return -1  # Not found
```

### Template 2: Boundary/Condition Search (Floor, Ceil, Lower Bound)
Used when searching for the *first* or *last* element satisfying a condition (e.g. first element $\ge$ target). We track a candidate variable `ans`.

```python
# Template 2: Boundary (e.g., Floor: largest value <= target)
low, high = 0, len(arr) - 1
ans = -1
while low <= high:
    mid = low + (high - low) // 2
    if arr[mid] <= target:
        ans = mid      # Record candidate index
        low = mid + 1  # Look for a larger valid value to the right
    else:
        high = mid - 1 # Too large, look left
return ans
```

### Template 3: Search on Answer Space
Used when solving word problems where the answer is a number in a known range (e.g., eating speed between $1$ and $10^9$).
```python
# Template 3: Search on Answer
low, high = MIN_POSSIBLE_ANSWER, MAX_POSSIBLE_ANSWER
ans = -1
while low <= high:
    mid = low + (high - low) // 2
    if is_valid_answer(mid):
        ans = mid      # Record valid answer
        high = mid - 1 # Try to find a smaller (better) valid answer
    else:
        low = mid + 1  # Search for larger speeds
return ans
```

---

## 🎯 Practice Problems & Worked Solutions

Practice these standard questions to master Binary Search.

| # | Problem | Difficulty | Link | Key Idea |
|---|---|---|---|---|
| 1 | Binary Search | Easy | [LeetCode](https://leetcode.com/problems/binary-search/) | Exact match search. |
| 2 | Search Insert Position | Easy | [LeetCode](https://leetcode.com/problems/search-insert-position/) | Finding insertion index. |
| 3 | First & Last Position | Medium | [LeetCode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | Two separate boundary searches. |
| 4 | Floor in a Sorted Array | Easy | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/floor-in-a-sorted-array-1587115620/1) | Find largest element $\le$ target. |
| 5 | Search in Rotated Sorted Array | Medium | [LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/) | Identify which half is sorted. |

---

### Solution 1: Binary Search (Exact Match)

**Complexity:**
- **Time:** $O(\log n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int search(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high) {
        // Safe middle calculation to prevent integer overflow
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            return mid; // Target found
        } else if (nums[mid] < target) {
            low = mid + 1; // Search right half
        } else {
            high = mid - 1; // Search left half
        }
    }
    return -1; // Not found
}
```

#### Python
```python
def search(nums, target):
    low, high = 0, len(nums) - 1
    while low <= high:
        # '//' is integer division in Python
        mid = low + (high - low) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1
```

#### C++
```cpp
int search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}
```
</details>

---

### Solution 2: Search Insert Position

**Intuition:** 
Run standard binary search. If the target is not found, the `low` pointer will end up pointing to the index where the target *should* be inserted to maintain sorted order.

**Complexity:**
- **Time:** $O(\log n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int searchInsert(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return low; // low represents the insertion position
}
```

#### Python
```python
def search_insert(nums, target):
    low, high = 0, len(nums) - 1
    while low <= high:
        mid = low + (high - low) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return low
```

#### C++
```cpp
int searchInsert(vector<int>& nums, int target) {
    int low = 0, high = nums.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return low;
}
```
</details>

---

### Solution 3: First & Last Position of Element in Sorted Array

**Intuition:**
Run binary search twice.
1. First run finds the left boundary (first occurrence). When `nums[mid] == target`, save `mid` as a candidate but continue searching *left* by setting `high = mid - 1`.
2. Second run finds the right boundary (last occurrence). When `nums[mid] == target`, save `mid` but continue searching *right* by setting `low = mid + 1`.

**Complexity:**
- **Time:** $O(\log n)$ — Two independent binary searches.
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] searchRange(int[] nums, int target) {
    int first = findBound(nums, target, true);
    int last = findBound(nums, target, false);
    return new int[]{first, last};
}

private int findBound(int[] nums, int target, boolean isFirst) {
    int low = 0, high = nums.length - 1;
    int ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            ans = mid; // Record candidate
            if (isFirst) {
                high = mid - 1; // Keep searching left
            } else {
                low = mid + 1;  // Keep searching right
            }
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```

#### Python
```python
def search_range(nums, target):
    def find_bound(is_first):
        low, high = 0, len(nums) - 1
        ans = -1
        while low <= high:
            mid = low + (high - low) // 2
            if nums[mid] == target:
                ans = mid
                if is_first:
                    high = mid - 1  # Keep looking left
                else:
                    low = mid + 1   # Keep looking right
            elif nums[mid] < target:
                low = mid + 1
            else:
                high = mid - 1
        return ans
    return [find_bound(True), find_bound(False)]
```

#### C++
```cpp
int findBound(vector<int>& nums, int target, bool isFirst) {
    int low = 0, high = nums.size() - 1;
    int ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            ans = mid;
            if (isFirst) {
                high = mid - 1; // Look left
            } else {
                low = mid + 1;  // Look right
            }
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}

vector<int> searchRange(vector<int>& nums, int target) {
    return {findBound(nums, target, true), findBound(nums, target, false)};
}
```
</details>

---

### Solution 4: Floor in a Sorted Array

**Intuition:**
Use Template 2.
We seek the largest index `i` such that `arr[i] <= x`.
- If `arr[mid] <= x`, then `mid` is a valid candidate for the floor. We store `ans = mid` and search the right half (`low = mid + 1`) to check if there is an even larger element that is still $\le x$.
- If `arr[mid] > x`, the element is too large. We search the left half (`high = mid - 1`).

**Complexity:**
- **Time:** $O(\log n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int findFloor(long[] arr, int n, long x) {
    int low = 0;
    int high = n - 1;
    int ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] <= x) {
            ans = mid;      // Valid floor candidate
            low = mid + 1;  // Try to find a larger value
        } else {
            high = mid - 1; // Element is too large, search left
        }
    }
    return ans;
}
```

#### Python
```python
def find_floor(arr, x):
    low, high = 0, len(arr) - 1
    ans = -1
    while low <= high:
        mid = low + (high - low) // 2
        if arr[mid] <= x:
            ans = mid
            low = mid + 1  # Seek larger valid candidates
        else:
            high = mid - 1 # Seek smaller values
    return ans
```

#### C++
```cpp
int findFloor(vector<long long>& arr, long long x) {
    int low = 0;
    int high = arr.size() - 1;
    int ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] <= x) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```
</details>

---

### Solution 5: Search in Rotated Sorted Array

**Intuition:**
A sorted array that has been rotated contains two sorted sub-sections (e.g. `[4, 5, 6, 7, 0, 1, 2]`).
If we choose a pivot index `mid`, **at least one of the halves (left or right) is guaranteed to be normally sorted**.
1. Find if the left half is sorted: `nums[low] <= nums[mid]`.
   - If yes: check if `target` lies inside the left half range (`nums[low] <= target < nums[mid]`). If it does, search left (`high = mid - 1`); otherwise search right (`low = mid + 1`).
2. If the left half is not sorted, the right half must be sorted.
   - Check if `target` lies inside the right half range (`nums[mid] < target <= nums[high]`). If it does, search right (`low = mid + 1`); otherwise search left (`high = mid - 1`).

**Complexity:**
- **Time:** $O(\log n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int search(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            return mid;
        }
        
        // 1. Check if Left Half is Sorted
        if (nums[low] <= nums[mid]) {
            // Check if target lies within the sorted left half
            if (nums[low] <= target && target < nums[mid]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        } 
        // 2. Otherwise, Right Half must be Sorted
        else {
            // Check if target lies within the sorted right half
            if (nums[mid] < target && target <= nums[high]) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
    }
    return -1;
}
```

#### Python
```python
def search(nums, target):
    low, high = 0, len(nums) - 1
    while low <= high:
        mid = low + (high - low) // 2
        if nums[mid] == target:
            return mid
            
        # Left half sorted
        if nums[low] <= nums[mid]:
            if nums[low] <= target < nums[mid]:
                high = mid - 1
            else:
                low = mid + 1
        # Right half sorted
        else:
            if nums[mid] < target <= nums[high]:
                low = mid + 1
            else:
                high = mid - 1
    return -1
```

#### C++
```cpp
int search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) return mid;
        
        if (nums[low] <= nums[mid]) {
            if (nums[low] <= target && target < nums[mid]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        } else {
            if (nums[mid] < target && target <= nums[high]) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
    }
    return -1;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `nums = [4, 5, 6, 7, 0, 1, 2]`, `target = 0`

| Iteration | `low` | `high` | `mid` | `nums[mid]` | Sorted Half | Range Check for Target `0` | Next Pointers |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 6 | 3 | 7 | Left (`nums[0] <= 7`) | `4 <= 0 < 7` (False) | `low = mid + 1 = 4` |
| 2 | 4 | 6 | 5 | 1 | Right (`nums[5] <= 2`) | `1 < 0 <= 2` (False) | `high = mid - 1 = 4` |
| 3 | 4 | 4 | 4 | 0 | Match! (`nums[4] == 0`) | Return index `4` | - |

**Returned Index:** `4` ✅
</details>

---

## 🎓 Viva Questions & Answers

### Q1: Why must the input array be sorted for Binary Search?
**Answer:**
Binary Search relies on the **Monotonic Property** (orderly increasing or decreasing sequence). This guarantees that comparing the middle element with the target deterministically eliminates half of the remaining search space. If unsorted, comparing `nums[mid]` gives no guarantee about where the target lies.

### Q2: Why do we use `mid = low + (high - low) / 2` instead of `mid = (low + high) / 2`?
**Answer:**
In languages with bounded integer types (like C, C++, Java), if `low` and `high` are large positive integers (e.g. near $2 \times 10^9$), `low + high` overflows the 32-bit signed integer capacity into a negative number, producing an incorrect negative index or crash. `low + (high - low) / 2` mathematically avoids sum overflow.

### Q3: What is the Lower Bound and Upper Bound in Binary Search?
**Answer:**
- **Lower Bound:** The index of the **first element** in a sorted array that is $\ge$ `target`.
- **Upper Bound:** The index of the **first element** in a sorted array that is strictly $>$ `target`.

### Q4: How does Binary Search work on Rotated Sorted Arrays?
**Answer:**
At any `mid` index in a rotated sorted array, at least one of the two halves (`[low...mid]` or `[mid...high]`) is guaranteed to be completely sorted. We identify which half is sorted, check if the target falls within that sorted half's range, and adjust `low` or `high` accordingly.

### Q5: What is "Search on Answer Space" (Binary Search on Answer)?
**Answer:**
When the problem output is a monotonic numerical value bounded in a range $[L, R]$ (e.g. minimum eating speed, minimum capacity), we apply Binary Search over the potential answer range rather than an array. For each candidate answer `mid`, we test validity with a helper function `isPossible(mid)`.

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Integer Overflow:**
   - Writing `(low + high) / 2` can cause an overflow error if `low + high` exceeds the maximum value of a 32-bit signed integer ($2 \times 10^9$).
   - Always use: `low + (high - low) / 2`.

2. **Off-by-One Infinite Loops:**
   - If you write `while (low < high)` instead of `while (low <= high)`, your loop may terminate early, missing the element when `low == high`.
   - If you update pointers as `low = mid` or `high = mid`, you can get stuck in an infinite loop because `mid` might not change value. Always shift past `mid`: `low = mid + 1` or `high = mid - 1`.

---

> 👉 Next, open `05-Strings.md` to learn how character sequences are handled and optimized! 💪

