# ➕ 08 — Prefix Sum

> **The central theme:**
> **Without prefix sums:** I keep recalculating addition from the past ($O(n)$ per query).
> **With prefix sums:** I remember previous additions and reuse them instantly ($O(1)$ per query).

---

## 🎯 Range Sum Queries: Why Prefix Sums Matter

Imagine you have an array `arr` and need to answer multiple queries of the form: **"What is the sum of elements from index $L$ to $R$?"**

- **The Brute Force Approach:** Iterate from $L$ to $R$ and add the elements. If there are $Q$ queries, this takes $O(Q \times n)$ time. If $n = 10^5$ and $Q = 10^5$, this is $10^{10}$ operations, which will crash (Time Limit Exceeded).
- **The Prefix Sum Approach:** 
  Precompute a running sum array $P$ where $P[i] = arr[0] + arr[1] + \dots + arr[i]$.
  
  $$\text{Sum}(L, R) = P[R] - P[L - 1] \quad (\text{if } L > 0)$$
  $$\text{Sum}(0, R) = P[R]$$

### Visualizing Range Sum Subtraction
To find the sum between indices $2$ and $4$ for the array `[1, 2, 3, 4, 5]`:

```
Array:    [  1,  2,  3,  4,  5  ]
Indices:     0   1   2   3   4
                     L───────►R
Prefix P: [  1,  3,  6, 10, 15  ]

Sum(2, 4) = P[4] - P[1]
          = 15 - 3 = 12
          (Indeed: 3 + 4 + 5 = 12)
```

We subtract the sum of elements before $L$ (which is $P[L-1]$) from the sum of elements up to $R$ (which is $P[R]$). This gives us the range sum in **$O(1)$ time**!

---

## 🎯 Practice Problems & Worked Solutions

Work through these prefix sum variants to build your pattern recognition.

| # | Problem | Difficulty | Link | Key Idea |
|---|---|---|---|---|
| 1 | Running Sum of 1D Array | Easy | [LeetCode](https://leetcode.com/problems/running-sum-of-1d-array/) | Build prefix sum in-place. |
| 2 | Find Highest Altitude | Easy | [LeetCode](https://leetcode.com/problems/find-the-highest-altitude/) | Track peak running sum. |
| 3 | Find Pivot Index | Easy | [LeetCode](https://leetcode.com/problems/find-pivot-index/) | balance left sum and right sum. |
| 4 | Left & Right Sum Differences | Easy | [LeetCode](https://leetcode.com/problems/left-and-right-sum-differences/) | Absolute difference at each pivot. |
| 5 | Minimum Start Value | Easy | [LeetCode](https://leetcode.com/problems/minimum-value-to-get-positive-step-by-step-sum/) | Find minimum running sum state. |
| 6 | Product of Array Except Self | Medium | [LeetCode](https://leetcode.com/problems/product-of-array-except-self/) | Prefix products $\times$ Suffix products. |

---

### Solution 1: Running Sum of 1D Array

**Complexity:**
- **Time:** $O(n)$ — Single pass.
- **Space:** $O(1)$ — Modifying the array in-place.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] runningSum(int[] nums) {
    for (int i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1]; // Accumulate previous sums
    }
    return nums;
}
```

#### Python
```python
def running_sum(nums):
    for i in range(1, len(nums)):
        nums[i] += nums[i - 1] # Accumulate previous sums
    return nums
```

#### C++
```cpp
vector<int> runningSum(vector<int>& nums) {
    for (int i = 1; i < nums.size(); i++) {
        nums[i] += nums[i - 1]; // Accumulate previous sums
    }
    return nums;
}
```
</details>

---

### Solution 2: Find the Highest Altitude

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int largestAltitude(int[] gain) {
    int currentAltitude = 0;
    int maxAltitude = 0;
    for (int g : gain) {
        currentAltitude += g;
        maxAltitude = Math.max(maxAltitude, currentAltitude);
    }
    return maxAltitude;
}
```

#### Python
```python
def largest_altitude(gain):
    current_altitude = 0
    max_altitude = 0
    for g in gain:
        current_altitude += g
        max_altitude = max(max_altitude, current_altitude)
    return max_altitude
```

#### C++
```cpp
int largestAltitude(vector<int>& gain) {
    int currentAltitude = 0;
    int maxAltitude = 0;
    for (int g : gain) {
        currentAltitude += g;
        maxAltitude = max(maxAltitude, currentAltitude);
    }
    return maxAltitude;
}
```
</details>

---

### Solution 3: Find Pivot Index

**Intuition:**
We need to find the index `i` where the sum of elements to the left equals the sum of elements to the right.
- Let the total sum of the array be `totalSum`.
- As we iterate, we maintain `leftSum`.
- The sum of elements to the right of `i` is simply:
  $$\text{rightSum} = \text{totalSum} - \text{leftSum} - arr[i]$$
- If `leftSum == rightSum`, then `i` is the pivot index.

**Complexity:**
- **Time:** $O(n)$ — Two passes (one for total sum, one to find pivot).
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int pivotIndex(int[] nums) {
    int totalSum = 0;
    for (int num : nums) {
        totalSum += num;
    }
    
    int leftSum = 0;
    for (int i = 0; i < nums.length; i++) {
        int rightSum = totalSum - leftSum - nums[i];
        if (leftSum == rightSum) {
            return i; // Found pivot index
        }
        leftSum += nums[i];
    }
    return -1;
}
```

#### Python
```python
def pivot_index(nums):
    total_sum = sum(nums)
    left_sum = 0
    for i, num in enumerate(nums):
        right_sum = total_sum - left_sum - num
        if left_sum == right_sum:
            return i
        left_sum += num
    return -1
```

#### C++
```cpp
int pivotIndex(vector<int>& nums) {
    int totalSum = 0;
    for (int num : nums) {
        totalSum += num;
    }
    
    int leftSum = 0;
    for (int i = 0; i < nums.size(); i++) {
        int rightSum = totalSum - leftSum - nums[i];
        if (leftSum == rightSum) {
            return i;
        }
        leftSum += nums[i];
    }
    return -1;
}
```
</details>

---

### Solution 4: Left and Right Sum Differences

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(n)$ to store the result array.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] leftRightDifference(int[] nums) {
    int totalSum = 0;
    for (int num : nums) {
        totalSum += num;
    }
    
    int leftSum = 0;
    int[] ans = new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        int rightSum = totalSum - leftSum - nums[i];
        ans[i] = Math.abs(leftSum - rightSum);
        leftSum += nums[i];
    }
    return ans;
}
```

#### Python
```python
def left_right_difference(nums):
    total_sum = sum(nums)
    left_sum = 0
    ans = []
    for num in nums:
        right_sum = total_sum - left_sum - num
        ans.append(abs(left_sum - right_sum))
        left_sum += num
    return ans
```

#### C++
```cpp
vector<int> leftRightDifference(vector<int>& nums) {
    int totalSum = 0;
    for (int num : nums) {
        totalSum += num;
    }
    
    int leftSum = 0;
    vector<int> ans(nums.size());
    for (int i = 0; i < nums.size(); i++) {
        int rightSum = totalSum - leftSum - nums[i];
        ans[i] = abs(leftSum - rightSum);
        leftSum += nums[i];
    }
    return ans;
}
```
</details>

---

### Solution 5: Minimum Start Value (Minimum Value for Positive Step sum)

**Intuition:**
We need the running sum to never fall below 1.
- Track the running prefix sum.
- Identify the minimum value `minPrefix` the running sum ever reaches.
- The minimum start value $X$ needed is:
  $$X = 1 - \text{minPrefix}$$
  *(For example, if the running sum drops to $-4$ at its lowest, you need a starting value of $1 - (-4) = 5$ to prevent it from dropping below 1).*

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int minStartValue(int[] nums) {
    int prefix = 0;
    int minPrefix = 0;
    for (int num : nums) {
        prefix += num;
        minPrefix = Math.min(minPrefix, prefix);
    }
    return 1 - minPrefix;
}
```

#### Python
```python
def min_start_value(nums):
    prefix = 0
    min_prefix = 0
    for num in nums:
        prefix += num
        min_prefix = min(min_prefix, prefix)
    return 1 - min_prefix
```

#### C++
```cpp
int minStartValue(vector<int>& nums) {
    int prefix = 0;
    int minPrefix = 0;
    for (int num : nums) {
        prefix += num;
        minPrefix = min(minPrefix, prefix);
    }
    return 1 - minPrefix;
}
```
</details>

---

### Solution 6: Product of Array Except Self

**Intuition:**
We cannot use division (e.g. dividing the total product by `nums[i]` is not allowed, and breaks if `nums[i]` is 0).
Instead, notice that for any index `i`, the product of all elements except `nums[i]` is:

$$\text{Product Except Self} = (\text{Product of all elements to the left}) \times (\text{Product of all elements to the right})$$

1. Create a result array `ans`. Set `ans[0] = 1`.
2. Do a left-to-right pass. Populate `ans[i]` with the product of all elements to the left of `i`: `ans[i] = ans[i-1] * nums[i-1]`.
3. Do a right-to-left pass. Maintain a running variable `right` (initially `1`). Multiply `ans[i]` by `right`, then update `right = right * nums[i]`.

**Complexity:**
- **Time:** $O(n)$ — Two passes.
- **Space:** $O(1)$ auxiliary space (excluding the output array).

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    
    // Step 1: Compute prefix (left) products in the ans array
    ans[0] = 1;
    for (int i = 1; i < n; i++) {
        ans[i] = ans[i - 1] * nums[i - 1];
    }
    
    // Step 2: Compute suffix (right) products and multiply on the fly
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        ans[i] = ans[i] * right; // left product * right product
        right = right * nums[i];  // update running suffix product
    }
    return ans;
}
```

#### Python
```python
def product_except_self(nums):
    n = len(nums)
    ans = [1] * n
    
    # Step 1: Compute left products
    for i in range(1, n):
        ans[i] = ans[i - 1] * nums[i - 1]
        
    # Step 2: Multiply by right products
    right = 1
    for i in range(n - 1, -1, -1):
        ans[i] = ans[i] * right
        right = right * nums[i]
        
    return ans
```

#### C++
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n);
    
    // Step 1: Compute left products
    ans[0] = 1;
    for (int i = 1; i < n; i++) {
        ans[i] = ans[i - 1] * nums[i - 1];
    }
    
    // Step 2: Multiply by right products
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        ans[i] = ans[i] * right;
        right = right * nums[i];
    }
    return ans;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `nums = [1, 2, 3, 4]`

- **Left pass (Prefix):**
  - `ans[0] = 1`
  - `ans[1] = ans[0] * nums[0] = 1 * 1 = 1`
  - `ans[2] = ans[1] * nums[1] = 1 * 2 = 2`
  - `ans[3] = ans[2] * nums[2] = 2 * 3 = 6`
  - `ans` state: `[1, 1, 2, 6]`

- **Right pass (Suffix):**
  - Initial `right = 1`
  - `i = 3`: `ans[3] = ans[3] * right = 6 * 1 = 6`. Update `right = 1 * 4 = 4`
  - `i = 2`: `ans[2] = ans[2] * right = 2 * 4 = 8`. Update `right = 4 * 3 = 12`
  - `i = 1`: `ans[1] = ans[1] * right = 1 * 12 = 12`. Update `right = 12 * 2 = 24`
  - `i = 0`: `ans[0] = ans[0] * right = 1 * 24 = 24`.

**Final output:** `[24, 12, 8, 6]` ✅
</details>

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Off-by-One on Range Sum Queries:**
   - When calculating `Sum(L, R)`, using `P[R] - P[L]` is a common mistake. If you do this, you subtract the element at index $L$, which should be included.
   - The correct formula is `P[R] - P[L-1]`.
   - Always handle the boundary case when $L = 0$ separately (since `P[-1]` is invalid), or declare your prefix array of size $n+1$ with `P[0] = 0` (1-based indexing).

2. **Integer Overflow:**
   - Cumulative sums grow rapidly. If your array has size $10^5$ and elements can be $10^5$, the prefix sum can reach $10^{10}$, which overflows a standard 32-bit signed integer. In Java/C++, use `long[]` prefix arrays to prevent overflow.

---

> 👉 Next, open `09-Sliding-Window.md` to learn how we track contiguous sub-arrays dynamically! 💪
