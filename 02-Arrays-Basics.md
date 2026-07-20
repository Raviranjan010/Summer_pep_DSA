# 📊 02 — Arrays: The Basics

> **The central concept:** An array is a collection of variables of the same type stored sequentially in memory. Master array traversal and you have unlocked half of DSA.

---

## 🧠 Deep Technical Dive: Memory & Cache Locality

An array is represented in RAM as a single, uninterrupted block of bytes.

### 1. Index Math
Because the memory is contiguous, the computer does not search for elements. It calculates their exact address in $O(1)$ time:

$$\text{Address}(arr[i]) = \text{Base Address} + i \times \text{Size of Element}$$

```
Index:         0           1           2           3
Values:    ┌───────────┬───────────┬───────────┬───────────┐
           │    10     │    20     │    30     │    40     │
           └───────────┴───────────┴───────────┴───────────┘
Address:   0x1000      0x1004      0x1008      0x1012  (assuming 4-byte integers)
```

### 2. Cache Friendliness (Spatial Locality)
CPUs have high-speed caches (L1, L2, L3) that are much faster than main RAM.
- When you access `arr[0]`, the CPU doesn't load *just* `arr[0]`. It loads a whole **cache line** (typically 64 bytes of consecutive memory) containing `arr[0]`, `arr[1]`, `arr[2]`, etc., into the cache.
- Therefore, iterating sequentially through an array is extremely fast because the next elements are already in the CPU cache. This is known as **spatial locality**.

### 3. Multi-Dimensional Arrays (2D Grids)
In memory, RAM is 1D. Therefore, a 2D array `arr[rows][cols]` must be flattened. Languages do this in one of two ways:
- **Row-Major Order (C++, Python, Java under-the-hood):** Rows are placed sequentially: Row 0, then Row 1, then Row 2.
  Formula: $\text{Address}(arr[i][j]) = \text{Base} + (i \times \text{cols} + j) \times \text{Size}$.
- **Column-Major Order (Fortran, MATLAB):** Columns are placed sequentially.

*Java note:* Java arrays are actually "arrays of arrays", meaning each row can reside in a different heap location. However, JVM optimizations keep them cache-friendly.

---

## 🛠️ Traversal & Search Operations

Let's look at the core mechanics of checking and scanning elements.

### 1. Finding the Largest Element
**Intuition:** Assume the element at index 0 is the largest. Walk through the array from index 1 to $n-1$. Whenever you encounter an element larger than our current champion, update the champion.

```
Array: [12, 35, 1, 10, 34, 1]
Assume max = 12.
- Check 35: 35 > 12 -> Update max = 35.
- Check 1: 1 < 35 -> No change.
- Check 10: 10 < 35 -> No change.
- Check 34: 34 < 35 -> No change.
- Check 1: 1 < 35 -> No change.
Result: 35.
```

### 2. Linear Search
**Intuition:** Start from index 0 and inspect every index sequentially. If the element matches the target, return the index. If we reach the end without finding the target, return `-1`.

---

## 🎯 Practice Problems & Worked Solutions

Here are 5 core practice problems to cement your understanding. Attempt them on LeetCode/GfG, then study the detailed solutions below.

| # | Problem | Difficulty | Link | Key Idea |
|---|---|---|---|---|
| 1 | Largest Element in Array | Easy | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/largest-element-in-array4009/0) | Find the maximum element. |
| 2 | Second Largest Element | Easy | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/second-largest3735/1) | Track two variables (largest and second). |
| 3 | Linear Search | Easy | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/who-will-win-1587115621/1) | Find target in an array. |
| 4 | Find Numbers with Even Number of Digits | Easy | [LeetCode](https://leetcode.com/problems/find-numbers-with-even-number-of-digits/) | Convert to string length or divide mathematically. |
| 5 | Running Sum of 1D Array | Easy | [LeetCode](https://leetcode.com/problems/running-sum-of-1d-array/) | Accumulate sum in place. |

---

### Solution 1: Largest Element in Array

**Complexity:**
- **Time Complexity:** $O(n)$ — We scan the array of size $n$ exactly once.
- **Space Complexity:** $O(1)$ — Only a single helper variable `max` is used.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int largest(int[] arr) {
    int max = arr[0]; // Assume first element is largest
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) {
            max = arr[i]; // Update largest
        }
    }
    return max;
}
```

#### Python
```python
def largest(arr):
    max_val = arr[0] # Assume first element is largest
    for i in range(1, len(arr)):
        if arr[i] > max_val:
            max_val = arr[i] # Update largest
    return max_val
```

#### C++
```cpp
int largest(vector<int>& arr) {
    int max_val = arr[0]; // Assume first element is largest
    for (int i = 1; i < arr.size(); i++) {
        if (arr[i] > max_val) {
            max_val = arr[i]; // Update largest
        }
    }
    return max_val;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `arr = [3, 8, 2, 11, 5]`

| Iteration | index `i` | `arr[i]` | Condition `arr[i] > max` | `max` Value |
| :---: | :---: | :---: | :---: | :---: |
| Initial | - | - | - | 3 |
| 1 | 1 | 8 | `8 > 3` (True) | 8 |
| 2 | 2 | 2 | `2 > 8` (False) | 8 |
| 3 | 3 | 11 | `11 > 8` (True) | 11 |
| 4 | 4 | 5 | `5 > 11` (False) | 11 |

**Returned Value:** 11 ✅
</details>

---

### Solution 2: Second Largest Element

**Intuition:**
We keep track of `largest` and `secondLargest`. Initially, both are `-1` (if array elements are positive integers). 
For each element `x` in the array:
1. If `x > largest`: `secondLargest` gets the old `largest`, and `largest` gets updated to `x`.
2. If `x < largest` and `x > secondLargest`: `second` gets updated to `x`.
3. If `x == largest`: Ignore it (we need the second *distinct* largest).

**Complexity:**
- **Time Complexity:** $O(n)$ — Single pass traversal.
- **Space Complexity:** $O(1)$ — Constant memory.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int secondLargest(int[] arr) {
    int largest = -1;
    int second = -1;
    for (int x : arr) {
        if (x > largest) {
            second = largest; // Old largest drops to second
            largest = x;      // Update largest
        } else if (x < largest && x > second) {
            second = x;       // New candidate for second largest
        }
    }
    return second;
}
```

#### Python
```python
def second_largest(arr):
    largest = -1
    second = -1
    for x in arr:
        if x > largest:
            second = largest # Old largest drops to second
            largest = x      # Update largest
        elif x < largest and x > second:
            second = x       # New candidate for second largest
    return second
```

#### C++
```cpp
int secondLargest(vector<int>& arr) {
    int largest = -1;
    int second = -1;
    for (int x : arr) {
        if (x > largest) {
            second = largest; // Old largest drops to second
            largest = x;      // Update largest
        } else if (x < largest && x > second) {
            second = x;       // New candidate for second largest
        }
    }
    return second;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `arr = [12, 35, 1, 10, 35, 34]`

| Step | Element `x` | `x > largest` | `x < largest && x > second` | `largest` | `second` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Init | - | - | - | -1 | -1 |
| 1 | 12 | True | - | 12 | -1 |
| 2 | 35 | True | - | 35 | 12 |
| 3 | 1 | False | `1 < 35 && 1 > 12` (False) | 35 | 12 |
| 4 | 10 | False | `10 < 35 && 10 > 12` (False) | 35 | 12 |
| 5 | 35 | False | `35 < 35 && 35 > 12` (False) | 35 | 12 |
| 6 | 34 | False | `34 < 35 && 34 > 12` (True) | 35 | 34 |

**Returned Value:** 34 ✅ (Correctly skipped duplicate 35)
</details>

---

### Solution 3: Linear Search

**Complexity:**
- **Time Complexity:** $O(n)$ — In the worst case (element not present or at the very end), we check all $n$ elements.
- **Space Complexity:** $O(1)$ — No extra space is allocated.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int search(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i; // Found at index i
        }
    }
    return -1; // Target not found
}
```

#### Python
```python
def search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i # Found at index i
    return -1 # Target not found
```

#### C++
```cpp
int search(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            return i; // Found at index i
        }
    }
    return -1; // Target not found
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `arr = [10, 20, 30, 40, 50]`, `target = 30`

| Iteration `i` | `arr[i]` | `arr[i] == target` | Action |
| :---: | :---: | :---: | :---: |
| 0 | 10 | `10 == 30` (False) | Continue loop |
| 1 | 20 | `20 == 30` (False) | Continue loop |
| 2 | 30 | `30 == 30` (True) | Return index `2` |

**Returned Value:** 2 ✅
</details>

---

### Solution 4: Find Numbers with Even Number of Digits

**Intuition:**
For each number in the array, determine the number of digits it has.
- Method A: Convert to string and find the string's length: `len(str(num))`.
- Method B: Repeatedly divide by 10 until the number becomes 0, counting the divisions.

**Complexity:**
- **Time Complexity:** $O(n \times d)$ where $d$ is the number of digits (typically $d \le 6$ for standard integer limits, so practically $O(n)$).
- **Space Complexity:** $O(1)$ auxiliary space (or $O(d)$ to store string representations temporarily).

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int findNumbers(int[] nums) {
    int count = 0;
    for (int n : nums) {
        if (getDigitCount(n) % 2 == 0) {
            count++;
        }
    }
    return count;
}

private int getDigitCount(int n) {
    int digits = 0;
    while (n > 0) {
        digits++;
        n /= 10;
    }
    return digits;
}
```

#### Python
```python
def find_numbers(nums):
    count = 0
    for n in nums:
        # Check digit count using string length
        if len(str(n)) % 2 == 0:
            count += 1
    return count
```

#### C++
```cpp
int findNumbers(vector<int>& nums) {
    int count = 0;
    for (int n : nums) {
        int digits = 0;
        int temp = n;
        while (temp > 0) {
            digits++;
            temp /= 10;
        }
        if (digits % 2 == 0) {
            count++;
        }
    }
    return count;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `nums = [555, 901, 4821, 1771]`

- `555`: Digits count = 3 (Odd) -> count remains 0.
- `901`: Digits count = 3 (Odd) -> count remains 0.
- `4821`: Digits count = 4 (Even) -> count becomes 1.
- `1771`: Digits count = 4 (Even) -> count becomes 2.

**Returned Value:** 2 ✅
</details>

---

### Solution 5: Running Sum of 1D Array

**Intuition:**
Instead of creating a new array, we can modify the array in-place to achieve $O(1)$ extra space.
For each index `i` from `1` to `n-1`, the running sum up to index `i` is the value of `nums[i]` plus the accumulated sum up to index `i-1` (which is already stored in `nums[i-1]`).

$$nums[i] = nums[i] + nums[i-1]$$

**Complexity:**
- **Time Complexity:** $O(n)$ — Single pass over the array.
- **Space Complexity:** $O(1)$ — If modifying the array in-place.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] runningSum(int[] nums) {
    for (int i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1]; // Accumulate previous sum in-place
    }
    return nums;
}
```

#### Python
```python
def running_sum(nums):
    for i in range(1, len(nums)):
        nums[i] += nums[i - 1] # Accumulate previous sum in-place
    return nums
```

#### C++
```cpp
vector<int> runningSum(vector<int>& nums) {
    for (int i = 1; i < nums.size(); i++) {
        nums[i] += nums[i - 1]; // Accumulate previous sum in-place
    }
    return nums;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `nums = [1, 2, 3, 4]`

- Initial state: `nums = [1, 2, 3, 4]`
- `i = 1`: `nums[1] = nums[1] + nums[0] = 2 + 1 = 3`. Array is now `[1, 3, 3, 4]`
- `i = 2`: `nums[2] = nums[2] + nums[1] = 3 + 3 = 6`. Array is now `[1, 3, 6, 4]`
- `i = 3`: `nums[3] = nums[3] + nums[2] = 4 + 6 = 10`. Array is now `[1, 3, 6, 10]`

**Result:** `[1, 3, 6, 10]` ✅
</details>

---

## ⚠️ Beginner Pitfalls & Common Bugs

1. **Index Out Of Bounds Exception:**
   - Always remember that the valid indices for an array of size $n$ are from $0$ to $n-1$.
   - A loop written as `for (int i = 0; i <= arr.length; i++)` will crash when $i = n$. Ensure your boundary is strictly less than length: `i < arr.length`.

2. **In-place Modification Side Effects:**
   - If you modify an array in-place, the original array data is overwritten. If you need the original values later in the algorithm, you must copy the array first.

3. **Wrong Initializations:**
   - When searching for a maximum value, do not initialize `max = 0` if the array can contain negative values (e.g., `[-10, -2, -5]`). Initialize `max = arr[0]` or `Integer.MIN_VALUE`.

---

## 🎓 Viva Questions & Answers

### Q1: Why is array element access $O(1)$ while searching for an unindexed element is $O(n)$?
**Answer:**
Array elements are stored in contiguous memory locations. Indexing uses direct arithmetic calculation $\text{Base} + (i \times \text{Size})$ to access any position in constant time $O(1)$. Searching requires inspecting elements one by one until a match is found, taking $O(n)$ time in the worst case.

### Q2: What is Spatial Locality and why does it make arrays faster than linked lists?
**Answer:**
Spatial Locality refers to the likelihood that adjacent memory locations will be accessed together. Arrays store elements contiguously, so loading an element fetches an entire CPU cache line containing surrounding elements into high-speed cache. Linked lists store nodes non-contiguously in heap memory, causing frequent CPU cache misses.

### Q3: What is Row-Major Order vs Column-Major Order in 2D Arrays?
**Answer:**
- **Row-Major Order:** Elements of each row are placed sequentially in 1D RAM memory (e.g. Row 0, Row 1, Row 2). Address formula: $\text{Base} + (i \times \text{cols} + j) \times \text{element\_size}$. Used in C++, Python, C.
- **Column-Major Order:** Elements of each column are placed sequentially in RAM (e.g. Col 0, Col 1, Col 2). Used in MATLAB, Fortran.

### Q4: Why is inserting or deleting an element in the middle of an array $O(n)$?
**Answer:**
Because arrays require contiguous memory alignment, inserting or deleting an element at index $k$ requires shifting all subsequent $n - 1 - k$ elements right or left by one position to preserve continuous layout.

### Q5: How do dynamic arrays (like Java `ArrayList` or C++ `std::vector`) handle resizing?
**Answer:**
When a dynamic array reaches full capacity, it allocates a new larger array (typically $1.5\times$ or $2\times$ the original capacity), copies all existing elements over, and frees the old array. Because resizing occurs infrequently, insertion has an **Amortized Time Complexity of $O(1)$**.

---

> 👉 Next, open `03-Two-Pointers.md` to see how we can optimize dual search variables! 💪

