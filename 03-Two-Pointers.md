# 🤝 03 — Two Pointers

> **Explain Like I'm 5:** Imagine two fingers pointing to elements in an array. Sometimes they start at opposite ends and walk towards the middle. Other times, one finger walks slowly while the other scans quickly ahead. 
> This simple strategy helps us avoid checking every single combination, optimizing our runtime from $O(n^2)$ to $O(n)$!

---

## 💡 The Core Intuition: Why does it work?

To understand why Two Pointers is so powerful, let's look at **Two Sum II (Pair with Target Sum in a Sorted Array)**.

### The Slow Way (Brute Force): $O(n^2)$
Check every possible pair:
`(index 0, index 1)`, `(index 0, index 2)`, ..., `(index 1, index 2)`, etc. 
This takes a nested loop, running in $O(n^2)$ time.

### The Fast Way (Two Pointers): $O(n)$
Since the array is **sorted**, we can place `left` at the beginning (smallest value) and `right` at the end (largest value).
```
Array: [1, 3, 5, 8, 10], Target = 13
Pointers: left (index 0, val 1), right (index 4, val 10)
```

1. **Current Sum:** `1 + 10 = 11`. 
   - Since `11 < 13` (too small), we need a larger sum.
   - Because the array is sorted, pairing `left` (value 1) with *any* other element to its left is impossible (there are none), and pairing it with elements to its right will yield smaller sums than if we used a larger element. Thus, **we can completely discard the element at `left`**.
   - Action: `left++` (move pointer to a larger value).
2. **Current Sum:** `3 + 10 = 13`.
   - Found target!

By skipping redundant checks, we examine each element at most once, reducing our time to $O(n)$.

---

## 🎨 The Two Pointer Templates

There are two main configurations of two-pointer techniques:

### Template 1: Opposite Ends (Meeting in the Middle)
Used for reversing arrays, checking palindromes, or finding pairs in sorted arrays.

```
[  X,  X,  X,  X,  X,  X  ]
   ▲                   ▲
  left               right
   └───►           ◄───┘
```

```python
# Opposite Ends Python Template
left, right = 0, len(arr) - 1
while left < right:
    # 1. Evaluate elements at arr[left] and arr[right]
    # 2. Shift pointers based on conditions
    if condition:
        left += 1
    else:
        right -= 1
```

### Template 2: Fast & Slow (Same Direction)
Used for in-place modifications (deleting duplicates, moving elements) or finding cycles in linked lists.

```
[  X,  X,  X,  X,  X,  X  ]
   ▲   ▲
 slow fast ───► (scans ahead)
```

```python
# Fast & Slow Python Template
slow = 0
for fast in range(len(arr)):
    if should_process(arr[fast]):
        # Swap or copy arr[fast] to arr[slow]
        arr[slow], arr[fast] = arr[fast], arr[slow]
        slow += 1
```

---

## 🎯 Practice Problems & Worked Solutions

Practice these problems in order. They represent the core two-pointer patterns.

| # | Problem | Difficulty | Link | Template Type |
|---|---|---|---|---|
| 1 | Reverse String | Easy | [LeetCode](https://leetcode.com/problems/reverse-string/) | Opposite Ends |
| 2 | Two Sum II (Sorted Array) | Medium | [LeetCode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | Opposite Ends |
| 3 | Squares of a Sorted Array | Easy | [LeetCode](https://leetcode.com/problems/squares-of-a-sorted-array/) | Opposite Ends (filling from back) |
| 4 | Move Zeroes | Easy | [LeetCode](https://leetcode.com/problems/move-zeroes/) | Fast & Slow |
| 5 | Remove Duplicates from Sorted Array | Easy | [LeetCode](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | Fast & Slow |

---

### Solution 1: Reverse String

**Intuition:** Swap the characters pointed to by `left` and `right`, then move `left` forward and `right` backward until they cross.

**Complexity:**
- **Time:** $O(n)$ — We inspect each character once.
- **Space:** $O(1)$ — Done in-place.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public void reverseString(char[] s) {
    int left = 0;
    int right = s.length - 1;
    while (left < right) {
        // Swap characters
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        // Move pointers inward
        left++;
        right--;
    }
}
```

#### Python
```python
def reverse_string(s): # s is a list of characters
    left, right = 0, len(s) - 1
    while left < right:
        # Swap characters in-place
        s[left], s[right] = s[right], s[left]
        # Move pointers inward
        left += 1
        right -= 1
```

#### C++
```cpp
void reverseString(vector<char>& s) {
    int left = 0;
    int right = s.size() - 1;
    while (left < right) {
        swap(s[left], s[right]); // Built-in utility swap
        left++;
        right--;
    }
}
```
</details>

---

### Solution 2: Two Sum II (Input Array Is Sorted)

**Intuition:** Since the indices must be 1-based, return `[left + 1, right + 1]`. Shrink search bounds according to the sum compared to the target.

**Complexity:**
- **Time:** $O(n)$ — Single pass scan.
- **Space:** $O(1)$ — No extra storage.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) {
            return new int[]{left + 1, right + 1}; // 1-based index
        } else if (sum < target) {
            left++;  // We need a larger value
        } else {
            right--; // We need a smaller value
        }
    }
    return new int[]{-1, -1};
}
```

#### Python
```python
def two_sum(numbers, target):
    left, right = 0, len(numbers) - 1
    while left < right:
        s = numbers[left] + numbers[right]
        if s == target:
            return [left + 1, right + 1]
        elif s < target:
            left += 1
        else:
            right -= 1
    return [-1, -1]
```

#### C++
```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    int left = 0;
    int right = numbers.size() - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) {
            return {left + 1, right + 1};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return {-1, -1};
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `numbers = [2, 7, 11, 15], target = 9`

- `left = 0` (value 2), `right = 3` (value 15)
- `sum = 2 + 15 = 17`. `17 > 9` -> `right--` (`right` becomes 2)
- `left = 0` (value 2), `right = 2` (value 11)
- `sum = 2 + 11 = 13`. `13 > 9` -> `right--` (`right` becomes 1)
- `left = 0` (value 2), `right = 1` (value 7)
- `sum = 2 + 7 = 9`. `9 == 9` -> returns `[1, 2]` ✅
</details>

---

### Solution 3: Squares of a Sorted Array

**Intuition:** 
Because the input can contain negative numbers (e.g. `[-4, -1, 0, 3, 10]`), the largest squares could reside at the beginning (e.g. $(-4)^2 = 16$) or at the end (e.g. $(10)^2 = 100$).
Place pointers at `left` and `right`. Compare their squared values. Write the larger square at the **back** of a new results array, then move that pointer inward.

**Complexity:**
- **Time:** $O(n)$ — Single pass to fill the new array.
- **Space:** $O(n)$ — To store the result.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] sortedSquares(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    int left = 0;
    int right = n - 1;
    int pos = n - 1; // Start filling from the back

    while (left <= right) {
        int leftSquare = nums[left] * nums[left];
        int rightSquare = nums[right] * nums[right];
        
        if (leftSquare > rightSquare) {
            result[pos] = leftSquare;
            left++;
        } else {
            result[pos] = rightSquare;
            right--;
        }
        pos--;
    }
    return result;
}
```

#### Python
```python
def sorted_squares(nums):
    n = len(nums)
    result = [0] * n
    left, right = 0, n - 1
    pos = n - 1 # Start filling from the back
    
    while left <= right:
        left_sq = nums[left] ** 2
        right_sq = nums[right] ** 2
        
        if left_sq > right_sq:
            result[pos] = left_sq
            left += 1
        else:
            result[pos] = right_sq
            right -= 1
        pos -= 1
    return result
```

#### C++
```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n);
    int left = 0;
    int right = n - 1;
    int pos = n - 1;
    
    while (left <= right) {
        int leftSquare = nums[left] * nums[left];
        int rightSquare = nums[right] * nums[right];
        
        if (leftSquare > rightSquare) {
            result[pos] = leftSquare;
            left++;
        } else {
            result[pos] = rightSquare;
            right--;
        }
        pos--;
    }
    return result;
}
```
</details>

---

### Solution 4: Move Zeroes

**Intuition:** 
Use the **Fast & Slow** template.
- The `slow` pointer marks the index where the next non-zero element should go.
- The `fast` pointer scans the array.
- When `fast` finds a non-zero element, swap the elements at `slow` and `fast`, then increment `slow`.

**Complexity:**
- **Time:** $O(n)$ — Single pass scan.
- **Space:** $O(1)$ — Modifies array in-place.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public void moveZeroes(int[] nums) {
    int slow = 0;
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != 0) {
            // Swap elements at slow and fast
            int temp = nums[slow];
            nums[slow] = nums[fast];
            nums[fast] = temp;
            
            slow++;
        }
    }
}
```

#### Python
```python
def move_zeroes(nums):
    slow = 0
    for fast in range(len(nums)):
        if nums[fast] != 0:
            # Swap elements in-place
            nums[slow], nums[fast] = nums[fast], nums[slow]
            slow += 1
```

#### C++
```cpp
void moveZeroes(vector<int>& nums) {
    int slow = 0;
    for (int fast = 0; fast < nums.size(); fast++) {
        if (nums[fast] != 0) {
            swap(nums[slow], nums[fast]);
            slow++;
        }
    }
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `nums = [0, 1, 0, 3, 12]`

- `slow = 0`, `fast = 0` (value 0). No swap.
- `slow = 0`, `fast = 1` (value 1). Swap `nums[0]` and `nums[1]`. Array: `[1, 0, 0, 3, 12]`, `slow` becomes 1.
- `slow = 1`, `fast = 2` (value 0). No swap.
- `slow = 1`, `fast = 3` (value 3). Swap `nums[1]` and `nums[3]`. Array: `[1, 3, 0, 0, 12]`, `slow` becomes 2.
- `slow = 2`, `fast = 4` (value 12). Swap `nums[2]` and `nums[4]`. Array: `[1, 3, 12, 0, 0]`, `slow` becomes 3.

**Result:** `[1, 3, 12, 0, 0]` ✅
</details>

---

### Solution 5: Remove Duplicates from Sorted Array

**Intuition:**
Since the array is sorted, duplicates are guaranteed to be adjacent.
- `slow` pointer points to the last known unique element.
- `fast` pointer scans the array starting from index 1.
- If `nums[fast] != nums[slow]`, it means we found a new unique element. We increment `slow`, and copy `nums[fast]` to `nums[slow]`.

**Complexity:**
- **Time:** $O(n)$ — Traverses the array once.
- **Space:** $O(1)$ — In-place.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int slow = 0;
    for (int fast = 1; fast < nums.length; fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast]; // Overwrite next position with unique element
        }
    }
    return slow + 1; // Length of the unique sub-array
}
```

#### Python
```python
def remove_duplicates(nums):
    if not nums:
        return 0
    slow = 0
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    return slow + 1
```

#### C++
```cpp
int removeDuplicates(vector<int>& nums) {
    if (nums.empty()) return 0;
    int slow = 0;
    for (int fast = 1; fast < nums.size(); fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    return slow + 1;
}
```
</details>

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Forgetting to Sort:**
   - The opposite-ends summation trick *only* works if the array is sorted. If it is unsorted, you must sort it first ($O(n \log n)$ time) or use a HashMap ($O(n)$ space).

2. **Pointer Index Bounds:**
   - In meeting-in-the-middle, the condition is usually `while left < right`. If you use `while left <= right`, the pointers might cross and cause duplicates or infinite loops.

3. **Writing to the Wrong Index in Fast & Slow:**
   - In "Remove Duplicates", make sure to increment `slow` *before* copying: `slow++; nums[slow] = nums[fast];`. Otherwise, you overwrite the current unique element.

---

> 👉 Next, open `04-Binary-Search.md` to see how we search sorted lists at lightning speed! 💪
