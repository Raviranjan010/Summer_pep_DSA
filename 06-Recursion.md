# 🔁 06 — Recursion

> **Explain Like I'm 5:** Stand between two mirrors facing each other. You see yourself, inside a smaller you, inside a smaller you… forever. Each reflection is the *same picture, just smaller.* That's recursion — **a function that calls a smaller copy of itself.**
>
> But mirrors going on forever never finish. So recursion needs a **wall to stop at.** That stopping point is called the **base case.**

---

## 💻 Under the Hood: The Execution Call Stack

To understand recursion, you must understand how the computer executes function calls.
Whenever a function is called, the computer allocates a block of memory called a **Stack Frame** (or Activation Record) on top of the **Call Stack**.

### What's inside a Stack Frame?
1. **Local Variables:** Variables declared inside the function.
2. **Parameters:** Values passed into the function (e.g. `n`).
3. **Return Address:** The line of code to jump back to once the function finishes.

### Visualizing Stack Frames for `factorial(3)`
Each function call pushes a new frame. They sit on top of each other. The computer can only interact with the frame on the very top.

```
Pushes (Calls)                          Pops (Returns)
┌───────────────────────┐               ┌───────────────────────┐
│ factorial(1) [n=1]    │               │ factorial(1) returns 1│ (Popped!)
├───────────────────────┤               ├───────────────────────┤
│ factorial(2) [n=2]    │               │ factorial(2) returns 2│ (2 * 1)
├───────────────────────┤               ├───────────────────────┤
│ factorial(3) [n=3]    │               │ factorial(3) returns 6│ (3 * 2)
└───────────────────────┘               └───────────────────────┘
     Call Stack (GROWING)                   Call Stack (SHRINKING)
```

If your code doesn't hit a base case, it keeps pushing frames until it runs out of memory, causing a **`StackOverflowError`**.

---

## 🧮 The Leap of Faith: Mathematical Induction

Writing recursion is about **trust**. Do not try to trace all the calls in your head. Instead, think of it like **Mathematical Induction**:

1. **Prove the Base Case:** Show that the code works for the smallest input (e.g. $n = 0$ or $n = 1$).
2. **Assume the Hypothesis:** Assume that the recursive call `solve(n - 1)` works correctly. (Do not trace it; trust that it returns the correct value.)
3. **Write the Inductive Step:** Use the result of `solve(n - 1)` to build the answer for `solve(n)`.

*Example (Factorial):*
- Base Case: `factorial(1) = 1`
- Hypothesis: Assume `factorial(n - 1)` correctly computes $(n-1)!$
- Inductive Step: `factorial(n) = n * factorial(n - 1)` (Correct!)

---

## 🪜 The Staircase Analogy: "Down vs. Up"

The position of your code relative to the recursive call determines when it runs:

- **Work written BEFORE the recursive call:** Executed on the way **DOWN** (top-to-bottom).
- **Work written AFTER the recursive call:** Postponed and executed on the way **UP** (bottom-to-top).

```
          [Call Stack Entry]
                  │
        (1. Work on way DOWN)
                  │
        [Recursive call solve(n-1)] ──► (Dives deeper)
                  │
        (2. Work on way UP)
                  │
         [Call Stack Exit]
```

---

## 🎨 Tree Recursion: Fibonacci

When a function calls itself **twice**, the execution path forms a **Recursion Tree** rather than a straight line.

```
                                 fib(4)
                               /        \
                            fib(3)      fib(2)  (Wasted duplicate work!)
                           /      \     /    \
                       fib(2)   fib(1) fib(1) fib(0)
                       /    \
                    fib(1) fib(0)
```

- **Time Complexity:** $O(2^n)$ — The tree roughly doubles in size at each level.
- **Space Complexity:** $O(n)$ — The maximum stack depth matches the height of the tree. Only one branch is stored in the call stack at any single moment.

---

## 🎯 Practice Problems & Worked Solutions

Verify your recursion logic with these 8 foundational problems.

| # | Problem | Type | Link | Key Idea |
|---|---|---|---|---|
| 1 | Print 1 to N | Linear (Up) | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/print-1-to-n-without-using-loops-1587115620/1) | Print after the recursive call. |
| 2 | Factorial | Linear (Return) | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/factorial5739/1) | $N \times (N-1)!$ |
| 3 | Sum of first N | Linear (Return) | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/sum-of-first-n-terms5843/1) | $N + Sum(N-1)$ |
| 4 | Power(x, n) | Linear/Log | [LeetCode](https://leetcode.com/problems/powx-n/) | Divide and conquer exponents. |
| 5 | Fibonacci Number | Tree | [LeetCode](https://leetcode.com/problems/fibonacci-number/) | Sum of two previous states. |
| 6 | Reverse a String | Linear | [LeetCode](https://leetcode.com/problems/reverse-string/) | `reverse(rest) + first`. |
| 7 | Valid Palindrome | Two-Pointer | [LeetCode](https://leetcode.com/problems/valid-palindrome/) | Pointers checking inward recursively. |
| 8 | Binary Search | Divide & Conquer | [LeetCode](https://leetcode.com/problems/binary-search/) | Search bounds halved recursively. |

---

### Solution 1: Print 1 to N

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(n)$ stack frames.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public void print1toN(int n) {
    if (n == 0) return; // Base case
    print1toN(n - 1);   // Go down the stack first
    System.out.println(n); // Print on the way UP
}
```

#### Python
```python
def print_1_to_n(n):
    if n == 0:
        return
    print_1_to_n(n - 1)  # Go down the stack first
    print(n)            # Print on the way UP
```

#### C++
```cpp
void print1toN(int n) {
    if (n == 0) return;
    print1toN(n - 1);   // Go down the stack first
    cout << n << endl;  // Print on the way UP
}
```
</details>

---

### Solution 2: Factorial

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(n)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int factorial(int n) {
    if (n <= 1) return 1; // Base case (prevents infinite loop for n=0)
    return n * factorial(n - 1); // Inductive step
}
```

#### Python
```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

#### C++
```cpp
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```
</details>

---

### Solution 3: Sum of First N

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(n)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int sum(int n) {
    if (n == 0) return 0;
    return n + sum(n - 1);
}
```

#### Python
```python
def sum_to_n(n):
    if n == 0:
        return 0
    return n + sum_to_n(n - 1)
```

#### C++
```cpp
int sum(int n) {
    if (n == 0) return 0;
    return n + sum(n - 1);
}
```
</details>

---

### Solution 4: Power(x, n) (Optimized Binary Exponentiation)

**Intuition:** 
Instead of calculating $x^n$ linearily ($O(n)$), we can divide the exponent in half at each step:
- If $n$ is even: $x^n = (x^{n/2})^2$
- If $n$ is odd: $x^n = x \times (x^{n/2})^2$
This reduces the time complexity from $O(n)$ to $O(\log n)$.

**Complexity:**
- **Time:** $O(\log n)$
- **Space:** $O(\log n)$ stack memory.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public double myPow(double x, int n) {
    long N = n;
    if (N < 0) {
        x = 1 / x;
        N = -N;
    }
    return fastPow(x, N);
}

private double fastPow(double x, long n) {
    if (n == 0) return 1.0;
    double half = fastPow(x, n / 2);
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * x;
    }
}
```

#### Python
```python
def my_pow(x, n):
    if n < 0:
        x = 1 / x
        n = -n
    
    def fast_pow(val, exp):
        if exp == 0:
            return 1.0
        half = fast_pow(val, exp // 2)
        if exp % 2 == 0:
            return half * half
        else:
            return half * half * val
            
    return fast_pow(x, n)
```

#### C++
```cpp
double fastPow(double x, long long n) {
    if (n == 0) return 1.0;
    double half = fastPow(x, n / 2);
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * x;
    }
}

double myPow(double x, int n) {
    long long N = n;
    if (N < 0) {
        x = 1 / x;
        N = -N;
    }
    return fastPow(x, N);
}
```
</details>

---

### Solution 5: Fibonacci Number

**Complexity:**
- **Time:** $O(2^n)$
- **Space:** $O(n)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```

#### Python
```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

#### C++
```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```
</details>

---

### Solution 6: Reverse a String

**Intuition:** 
Reverse the substring starting at index 1, and append the first character to the end of that reversed substring.
`reverse("abc")` = `reverse("bc")` + `'a'`.

**Complexity:**
- **Time:** $O(n^2)$ (due to string slicing and copying in Java/Python).
- **Space:** $O(n)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public String reverse(String s) {
    if (s.length() <= 1) return s;
    return reverse(s.substring(1)) + s.charAt(0);
}
```

#### Python
```python
def reverse(s):
    if len(s) <= 1:
        return s
    return reverse(s[1:]) + s[0]
```

#### C++
```cpp
string reverseString(string s) {
    if (s.length() <= 1) return s;
    return reverseString(s.substr(1)) + s[0];
}
```
</details>

---

### Solution 7: Valid Palindrome (Recursive Two-Pointer)

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(n)$ stack frames.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean isPalindrome(String s) {
    return checkPalindrome(s, 0, s.length() - 1);
}

private boolean checkPalindrome(String s, int left, int right) {
    if (left >= right) return true;
    if (s.charAt(left) != s.charAt(right)) return false;
    return checkPalindrome(s, left + 1, right - 1);
}
```

#### Python
```python
def is_palindrome(s):
    def check_palindrome(left, right):
        if left >= right:
            return True
        if s[left] != s[right]:
            return False
        return check_palindrome(left + 1, right - 1)
        
    return check_palindrome(0, len(s) - 1)
```

#### C++
```cpp
bool checkPalindrome(const string& s, int left, int right) {
    if (left >= right) return true;
    if (s[left] != s[right]) return false;
    return checkPalindrome(s, left + 1, right - 1);
}

bool isPalindrome(string s) {
    return checkPalindrome(s, 0, s.size() - 1);
}
```
</details>

---

### Solution 8: Binary Search (Recursive)

**Complexity:**
- **Time:** $O(\log n)$
- **Space:** $O(\log n)$ stack frames.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int search(int[] nums, int target) {
    return binarySearch(nums, 0, nums.length - 1, target);
}

private int binarySearch(int[] nums, int low, int high, int target) {
    if (low > high) return -1;
    int mid = low + (high - low) / 2;
    if (nums[mid] == target) return mid;
    if (nums[mid] < target) {
        return binarySearch(nums, mid + 1, high, target);
    } else {
        return binarySearch(nums, low, mid - 1, target);
    }
}
```

#### Python
```python
def search(nums, target):
    def binary_search(low, high):
        if low > high:
            return -1
        mid = low + (high - low) // 2
        if nums[mid] == target:
            return mid
        if nums[mid] < target:
            return binary_search(mid + 1, high)
        else:
            return binary_search(low, mid - 1)
            
    return binary_search(0, len(nums) - 1)
```

#### C++
```cpp
int binarySearch(const vector<int>& nums, int low, int high, int target) {
    if (low > high) return -1;
    int mid = low + (high - low) / 2;
    if (nums[mid] == target) return mid;
    if (nums[mid] < target) {
        return binarySearch(nums, mid + 1, high, target);
    } else {
        return binarySearch(nums, low, mid - 1, target);
    }
}

int search(vector<int>& nums, int target) {
    return binarySearch(nums, 0, nums.size() - 1, target);
}
```
</details>

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Incorrect/Missing Base Case:**
   - If the base case is missing or cannot be reached, the code loops forever until it throws a `StackOverflowError`. Always verify that every path moves closer to the base case.

2. **Tail Recursion & Memory:**
   - In some languages like C++, tail-call optimization can reuse stack frames for functions that return the result of their recursive call directly (e.g. Solution 8). However, Java and Python do not support this optimization natively. Recursion is always constrained by the stack depth limit (typically ~1000 in Python).

---

> 👉 Next, open `07-Hashing.md` to learn how we index keys and values in constant time! 💪
