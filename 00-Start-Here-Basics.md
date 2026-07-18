# 🧱 00 — Start Here: Absolute Basics

> If words like *array*, *loop*, or *time complexity* feel scary — this is the perfect place to begin. Read slowly. This is the foundation everything else stands on.

---

## 🍳 What is an Algorithm? (Explain Like I'm 5)

Imagine you want to make tea:
1. **Boil water**
2. **Add tea leaves**
3. **Add milk**
4. **Add sugar**
5. **Strain and serve**

If you swap step 1 and step 5, you get hot water poured over dry tea leaves in a strainer, which fails! The *order* and *logic* of the steps matter.

An **algorithm** is simply a sequence of well-defined steps to solve a problem. In Computer Science, Data Structures and Algorithms (DSA) is just learning the smartest, most efficient steps to solve computer-based problems.

---

## 📦 What is a Data Structure? (Explain Like I'm 5)

How you store your belongings determines how quickly you can find them:
- **Loose socks in a giant drawer:** Finding a matching pair takes a long time.
- **Socks organized in labeled boxes:** Finding a matching pair is instantaneous.

A **data structure** is a specific way of organizing and storing data in a computer so that we can access and modify it efficiently. An *array* is like the labeled box; a *linked list* is like a treasure hunt of clues. We will learn each one, step-by-step.

---

## 🧠 Memory Layout: How the Computer Thinks

Before writing code, you must understand where variables live. Computers use two primary areas of RAM: the **Stack** and the **Heap**.

### 1. The Stack (Fast & Organized)
- Stores primitive variables (like `int`, `char`, `double`, `boolean`) and local function parameters.
- Works like a stack of plates: the last item placed on the stack is the first one removed (LIFO).
- Highly organized, but size must be known at compile time.

### 2. The Heap (Large & Flexible)
- Stores complex objects (like Arrays, Lists, Objects, and Strings).
- Dynamically allocated at runtime.
- Variables on the **Stack** hold "references" (memory addresses/pointers) that point to the actual data located on the **Heap**.

### Visualizing Stack vs Heap
```
STACK:                                HEAP:
┌──────────────┬─────────────┐        ┌────────────────┬────────────────┐
│ Variable     │ Value/Ref   │        │ Memory Address │ Actual Data    │
├──────────────┼─────────────┤        ├────────────────┼────────────────┤
│ x (int)      │ 5           │        │                │                │
│ y (char)     │ 'A'         │        │                │                │
│ arr (int[])  │ 0x7ffd9     ├───────►│ 0x7ffd9        │ [10, 20, 30]   │
└──────────────┴─────────────┘        └────────────────┴────────────────┘
```

---

## 🔢 What is an Array in Memory?

An array is a collection of elements of the same type stored in **contiguous** (adjacent) memory locations. 

### Why do arrays start at index 0?
In low-level memory, the index represents the **offset** (distance) from the start of the array.
If the starting memory address (Base Address) of an array `arr` is `2000`, and each integer takes `4 bytes`:

$$\text{Address of } arr[i] = \text{Base Address} + (i \times \text{Size of Type})$$

```
Index:         0           1           2           3
Values:    ┌───────────┬───────────┬───────────┬───────────┐
           │    10     │    20     │    30     │    40     │
           └───────────┴───────────┴───────────┴───────────┘
Address:   2000        2004        2008        2012
Offset:    +0 bytes    +4 bytes    +8 bytes    +12 bytes
```

- To read $arr[0]$: $\text{Address} = 2000 + (0 \times 4) = 2000$ (Instant!)
- To read $arr[3]$: $\text{Address} = 2000 + (3 \times 4) = 2012$ (Instant!)

Because it uses simple addition and multiplication, accessing *any* index in an array takes the exact same amount of time: **constant time, or $O(1)$**.

---

## ⏱️ What is Time Complexity / Big-O Notation?

Big-O answers one crucial question:
> **"If my input size $n$ gets larger, how does the number of steps in my code grow?"**

We don't measure time in seconds (because a supercomputer is faster than a laptop). Instead, we measure the **growth of steps**.

### The Growth Hierarchy (From Best to Worst)

```
Steps (Time)
  ▲                                                  / O(n!) [Factorial]
  │                                                 /
  │                                                /  / O(2^n) [Exponential]
  │                                               /  /
  │                                              /  /   / O(n²) [Quadratic]
  │                                             /  /   /
  │                                            /  /   /    / O(n log n) [Linearithmic]
  │                                           /  /   /    /
  │                                          /  /   /    /      / O(n) [Linear]
  │                                         /  /   /    /      /
  │                                        /  /   /    /      /        / O(log n) [Logarithmic]
  │  ──────────────────────────────────────┴──┴───┴────┴──────┴────────┴─────────► O(1) [Constant]
  └─────────────────────────────────────────────────────────────────────────────► Input Size (n)
```

| Notation | Name | Plain-English Meaning | Common Code Example |
| :--- | :--- | :--- | :--- |
| **$O(1)$** | Constant | Same number of steps, regardless of input size. | Finding if a number is even/odd; Reading an array index. |
| **$O(\log n)$** | Logarithmic | Input is cut in half at every step. Extremely fast. | Binary Search. |
| **$O(n)$** | Linear | Steps grow proportionally to the input size. | Single loop scanning an array. |
| **$O(n \log n)$**| Linearithmic| Linear scan combined with logarithmic division. | Optimized Sorting (Merge Sort, Quick Sort). |
| **$O(n^2)$** | Quadratic | Steps grow as the square of the input size. | Nested loops (comparing every item to every other item). |
| **$O(2^n)$** | Exponential | Steps double with each additional input element. | Recursive Fibonacci without optimization. |
| **$O(n!)$** | Factorial | Growth matches all possible permutations. Avoid. | Generating all permutations of a string. |

---

## 🧠 Deep-Dive: Big-O Examples in Code

### 1. Constant Time: $O(1)$
No matter how large `n` becomes, this code always executes exactly 2 steps.
```python
def check_first_element(arr):
    first = arr[0]             # Step 1
    if first % 2 == 0:         # Step 2
        return "Even"
    return "Odd"
```

### 2. Logarithmic Time: $O(\log n)$
Every iteration divides the variable `n` by 2.
For $n = 16$, the loop runs $16 \to 8 \to 4 \to 2 \to 1$ (4 times). Since $2^4 = 16$, $\log_2(16) = 4$.
```python
def divide_by_two(n):
    steps = 0
    while n > 1:
        n = n // 2             # Division cuts search space in half
        steps += 1
    return steps
```

### 3. Linear Time: $O(n)$
If the array has 10 elements, the loop runs 10 times. If it has 1,000,000 elements, it runs 1,000,000 times.
```python
def print_all_elements(arr):
    for x in arr:              # Runs 'n' times
        print(x)
```

### 4. Quadratic Time: $O(n^2)$
An outer loop runs `n` times, and for *each* run, an inner loop runs `n` times. Total steps = $n \times n = n^2$.
```python
def find_all_pairs(arr):
    n = len(arr)
    for i in range(n):         # Runs 'n' times
        for j in range(n):     # Runs 'n' times for each 'i'
            print(arr[i], arr[j])
```

---

## 💾 What is Space Complexity?

Space complexity measures the **extra memory** (auxiliary space) your algorithm needs as input size $n$ grows.

- **$O(1)$ Extra Space:** You reuse existing memory or only create a few simple variables.
  ```python
  def find_sum(arr):
      total = 0                # Only 1 extra integer variable
      for num in arr:
          total += num
      return total
  ```
- **$O(n)$ Extra Space:** You create a new array/list containing size proportional to the input.
  ```python
  def copy_array(arr):
      new_arr = []             # Creating a brand new list
      for num in arr:
          new_arr.append(num)  # Will hold 'n' elements at the end
      return new_arr
  ```

---

## ⭐ The 4-Question Ritual (Memorize This)

Whenever you face a coding problem in an interview or online test, answer these four questions in order:
1. **What is the Brute Force solution?**
   - What is the simplest, most obvious way to solve this, even if it is slow? (Always start here. Working code is step 1.)
2. **Can we optimize it?**
   - Can we sort the input? Can we use extra memory to save lookup time? Can we use Two Pointers?
3. **What is the Time Complexity of my solution?**
   - Expressed in Big-O.
4. **What is the Space Complexity of my solution?**
   - Does it use extra memory?

---

## 📝 Practice: Big-O Analysis

Analyze the Time and Space complexity of the following code snippets. Try them yourself before reading the explanation!

### Question 1
```python
def process_data(arr):
    n = len(arr)
    sum_val = 0
    for i in range(n):
        sum_val += arr[i]
    
    for i in range(n):
        print(arr[i])
```
<details>
<summary>👀 Reveal Answer & Explanation</summary>

- **Time Complexity:** $O(n)$
  - *Explanation:* The first loop runs $n$ times. The second loop runs $n$ times. Total steps = $n + n = 2n$. In Big-O, we drop constants because we only care about growth trends. Hence, $O(2n) \to O(n)$.
- **Space Complexity:** $O(1)$
  - *Explanation:* We only created `sum_val` and loop counter variables. No new collections or recursion stacks were allocated.
</details>

### Question 2
```python
def print_grid(n):
    for i in range(n):
        for j in range(5):
            print(i, j)
```
<details>
<summary>👀 Reveal Answer & Explanation</summary>

- **Time Complexity:** $O(n)$
  - *Explanation:* The outer loop runs $n$ times. The inner loop always runs exactly 5 times regardless of $n$. Total steps = $n \times 5 = 5n$. Dropping constants, we get $O(n)$.
- **Space Complexity:** $O(1)$
</details>

### Question 3
```python
def build_lookup_table(arr):
    n = len(arr)
    seen_elements = set()
    for x in arr:
        seen_elements.add(x)
    return seen_elements
```
<details>
<summary>👀 Reveal Answer & Explanation</summary>

- **Time Complexity:** $O(n)$
  - *Explanation:* We loop through the array once ($n$ iterations). Adding an item to a Set is an average $O(1)$ operation. Total time = $n \times O(1) = O(n)$.
- **Space Complexity:** $O(n)$
  - *Explanation:* In the worst-case (all elements are unique), the `seen_elements` set will store $n$ values, which scales linearly with the input size.
</details>

---

## 🎯 Warm-Up Coding Exercises

Practice writing these basic algorithms in your language of choice. Try to figure out their Time and Space complexity.

### 1. Sum of First N Numbers
Write a function that calculates the sum of all integers from $1$ to $N$.
- **Formula approach:** $Sum = \frac{N \times (N+1)}{2}$
- **Loop approach:** Add $1, 2, \dots, N$ in a loop.

<details>
<summary>☕ Java Solution</summary>

```java
// Loop Approach: Time O(N), Space O(1)
int loopSum(int n) {
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += i;
    }
    return sum;
}

// Formula Approach: Time O(1), Space O(1)
int formulaSum(int n) {
    return n * (n + 1) / 2;
}
```
</details>

<details>
<summary>🐍 Python Solution</summary>

```python
# Loop Approach: Time O(N), Space O(1)
def loop_sum(n):
    total = 0
    for i in range(1, n + 1):
        total += i
    return total

# Formula Approach: Time O(1), Space O(1)
def formula_sum(n):
    return n * (n + 1) // 2
```
</details>

<details>
<summary>⚡ C++ Solution</summary>

```cpp
// Loop Approach: Time O(N), Space O(1)
int loopSum(int n) {
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += i;
    }
    return sum;
}

// Formula Approach: Time O(1), Space O(1)
int formulaSum(int n) {
    return n * (n + 1) / 2;
}
```
</details>

---

## Mini Glossary (Bookmark This)

| Term | Plain-English Meaning |
| :--- | :--- |
| **Index** | Box number in an array. Always starts at **0**. |
| **Iterate** | Loop through a collection of items one by one. |
| **Pointer** | A variable containing a position or address (like index variables `left` or `right`). |
| **Brute Force** | The simplest correct solution, even if inefficient. |
| **Optimize** | Redesigning code to make it run faster or consume less memory. |
| **Dry Run** | Tracing through code manually using pencil and paper, writing down variable values step-by-step. |
| **In-place** | Modifying the input data structure directly without using large extra helper structures (meaning $O(1)$ auxiliary space). |

---

> 👉 Once these basics feel clear, open `01-Git-And-GitHub.md` to learn how to store and track your code safely! 💪
