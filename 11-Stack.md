# 🥞 11 — Stack

> 🍽️ **Explain Like I'm 5:** A stack of plates. You can only take the **top** plate off, and you can only put a new plate **on top**. You can never grab a plate from the middle without removing everything above it first.
>
> That's a **Stack** — **LIFO: Last In, First Out.** The last plate you put on is the first one you take off.

```
Top
 ↓
[30]
[20]
[10]
```

---

## The 5 Operations (Memorize These Names)

| Operation | What it does |
|-----------|-------------|
| **Push** | Insert a new item on top |
| **Pop** | Remove the top item |
| **Peek** | Look at the top item (without removing it) |
| **isEmpty** | Check if the stack has nothing in it |
| **isFull** | Check if there's no more space (array version only) |

> 🔑 **Everything about stacks is just these 5 words.** Once they're automatic, every stack problem becomes "which of these 5 do I need right now?"

---

## Implementation 1 — Stack Using Array

> 🧠 **Teaching line: Stack = Array + Top Pointer.**
> The array holds the values. A `top` variable tracks the index of the current top item.

```
Top
 ↓
30   ← index 2
20   ← index 1
10   ← index 0
```

<details>
<summary>☕ Java</summary>

```java
class ArrayStack {
    int[] arr;
    int top;

    ArrayStack(int size) {
        arr = new int[size];
        top = -1;                       // -1 means empty
    }

    boolean isFull() {
        return top == arr.length - 1;
    }

    boolean isEmpty() {
        return top == -1;
    }

    void push(int x) {
        if (isFull()) {
            System.out.println("Stack Overflow");
            return;
        }
        arr[++top] = x;                 // move top up, then insert
    }

    int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow");
            return -1;
        }
        return arr[top--];               // read top, then move down
    }

    int peek() {
        if (isEmpty()) return -1;
        return arr[top];
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class ArrayStack:
    def __init__(self, size):
        self.arr = [0] * size
        self.top = -1                    # -1 means empty

    def is_full(self):
        return self.top == len(self.arr) - 1

    def is_empty(self):
        return self.top == -1

    def push(self, x):
        if self.is_full():
            print("Stack Overflow")
            return
        self.top += 1
        self.arr[self.top] = x           # move top up, then insert

    def pop(self):
        if self.is_empty():
            print("Stack Underflow")
            return -1
        val = self.arr[self.top]         # read top
        self.top -= 1                    # then move down
        return val

    def peek(self):
        if self.is_empty():
            return -1
        return self.arr[self.top]
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class ArrayStack {
    vector<int> arr;
    int top;
    int capacity;

public:
    ArrayStack(int size) {
        arr.resize(size);
        capacity = size;
        top = -1;                        // -1 means empty
    }

    bool isFull() { return top == capacity - 1; }
    bool isEmpty() { return top == -1; }

    void push(int x) {
        if (isFull()) {
            cout << "Stack Overflow" << endl;
            return;
        }
        arr[++top] = x;                  // move top up, then insert
    }

    int pop() {
        if (isEmpty()) {
            cout << "Stack Underflow" << endl;
            return -1;
        }
        return arr[top--];               // read top, then move down
    }

    int peek() {
        if (isEmpty()) return -1;
        return arr[top];
    }
};
```
</details>

> ⚠️ **Array stacks have a fixed size** — decided when you create them. Push past the limit → **Stack Overflow.**

> ⏱️ All operations: Time O(1) · Space O(n) for the array

---

## Implementation 2 — Stack Using Linked List

> 🧠 **Teaching line: Array Stack = Fixed Size. Linked List Stack = Dynamic Size.**
> No size limit — push adds a new node **at the head**, pop removes **from the head**. (The head IS the top — no walking required, so it's still O(1).)

```
Top
 ↓
[30] → [20] → [10] → null
```

<details>
<summary>☕ Java</summary>

```java
class Node {
    int val;
    Node next;
    Node(int val) { this.val = val; }
}

class LinkedListStack {
    Node top;

    boolean isEmpty() {
        return top == null;
    }

    void push(int x) {
        Node newNode = new Node(x);
        newNode.next = top;              // new node points to old top
        top = newNode;                   // new node becomes the top
    }

    int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow");
            return -1;
        }
        int val = top.val;
        top = top.next;                  // top moves down to next node
        return val;
    }

    int peek() {
        if (isEmpty()) return -1;
        return top.val;
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None

class LinkedListStack:
    def __init__(self):
        self.top = None

    def is_empty(self):
        return self.top is None

    def push(self, x):
        new_node = Node(x)
        new_node.next = self.top         # new node points to old top
        self.top = new_node              # new node becomes the top

    def pop(self):
        if self.is_empty():
            print("Stack Underflow")
            return -1
        val = self.top.val
        self.top = self.top.next         # top moves down to next node
        return val

    def peek(self):
        if self.is_empty():
            return -1
        return self.top.val
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
struct Node {
    int val;
    Node* next;
    Node(int val) { this->val = val; this->next = nullptr; }
};

class LinkedListStack {
    Node* top = nullptr;

public:
    bool isEmpty() { return top == nullptr; }

    void push(int x) {
        Node* newNode = new Node(x);
        newNode->next = top;             // new node points to old top
        top = newNode;                   // new node becomes the top
    }

    int pop() {
        if (isEmpty()) {
            cout << "Stack Underflow" << endl;
            return -1;
        }
        int val = top->val;
        Node* temp = top;
        top = top->next;                 // top moves down to next node
        delete temp;
        return val;
    }

    int peek() {
        if (isEmpty()) return -1;
        return top->val;
    }
};
```
</details>

> ⏱️ All operations: Time O(1) · Space O(n)

---

## In Real Code, Use the Built-In Stack

> You'll build one by hand once (above) to understand it. After that, use your language's built-in stack:

<details>
<summary>☕ Java</summary>

```java
Deque<Integer> stack = new ArrayDeque<>();  // preferred over old Stack class
stack.push(10);
stack.pop();
stack.peek();
stack.isEmpty();
```
</details>

<details>
<summary>🐍 Python</summary>

```python
stack = []                # a plain list works as a stack!
stack.append(10)          # push
stack.pop()                # pop
stack[-1]                  # peek
len(stack) == 0             # isEmpty
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
stack<int> st;
st.push(10);
st.pop();
st.top();                  // peek
st.empty();                 // isEmpty
```
</details>

---

## 📋 Implementation Comparison

| | Array Stack | Linked List Stack |
|---|-------------|-------------------|
| Size | **Fixed** (decided upfront) | **Dynamic** (grows as needed) |
| Can overflow? | Yes | No (until memory runs out) |
| Speed | O(1) | O(1) |
| Extra memory per item | None | A `next` pointer per node |

---

# 🎯 Applications — Where Stacks Actually Get Used

> 🪄 **The #1 clue a problem wants a stack:** anything about **matching pairs**, **undo history**, or **"most recent thing first."**

---

## Problem 1 — Valid Parentheses `(LC 20)` ⭐

> **Teaching line: Every opening bracket needs a matching closing bracket.**
> **Pattern:** Opening bracket → **push** it. Closing bracket → check if it **matches** the top, then **pop**.

```
"()[]{}"  → valid ✅
"([)]"    → invalid ❌ (brackets cross instead of nesting properly)
```

**Dry run — `"([)]"` (the invalid one):**

```
'(' → push        stack: (
'[' → push        stack: ( [
')' → closing! peek top = '[' → doesn't match ')' → INVALID ❌
```

<details>
<summary>☕ Java</summary>

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') {
            stack.push(c);                        // opening → push
        } else {
            if (stack.isEmpty()) return false;     // closing with nothing to match
            char top = stack.pop();
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }
    return stack.isEmpty();                        // nothing left unmatched
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def isValid(self, s):
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}

    for c in s:
        if c in '([{':
            stack.append(c)                        # opening → push
        else:
            if not stack or stack[-1] != pairs[c]:
                return False                        # no match
            stack.pop()

    return len(stack) == 0                          # nothing left unmatched
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') {
            st.push(c);                             // opening → push
        } else {
            if (st.empty()) return false;           // closing with nothing to match
            char top = st.top(); st.pop();
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }
    return st.empty();                              // nothing left unmatched
}
```
</details>

> ⏱️ Time O(n) · Space O(n)

---

## Problem 2 — Minimum Add to Make Parentheses Valid `(LC 921)`

> **Teaching line: Count unmatched brackets.**
> Why after LC 20? Because students already understand "unmatched" from the last problem — this just **counts** instead of returning true/false.

```
"())"  → answer: 1   (need 1 more '(' to fix the extra ')')
```

**Dry run:**
```
'(' → push        openNeeded = 1
')' → matches!    openNeeded = 0
')' → nothing to match! → closeNeeded++ (closeNeeded = 1)

Total additions needed = openNeeded + closeNeeded = 0 + 1 = 1  ✅
```

<details>
<summary>☕ Java</summary>

```java
public int minAddToMakeValid(String s) {
    int openNeeded = 0;    // unmatched '(' we're still waiting to close
    int closeNeeded = 0;   // unmatched ')' with nothing to match

    for (char c : s.toCharArray()) {
        if (c == '(') {
            openNeeded++;
        } else {
            if (openNeeded > 0) openNeeded--;   // this ')' matches an earlier '('
            else closeNeeded++;                 // no '(' waiting → extra ')'
        }
    }
    return openNeeded + closeNeeded;             // total brackets we must add
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def minAddToMakeValid(self, s):
    open_needed = 0        # unmatched '(' we're still waiting to close
    close_needed = 0       # unmatched ')' with nothing to match

    for c in s:
        if c == '(':
            open_needed += 1
        else:
            if open_needed > 0:
                open_needed -= 1                 # matches an earlier '('
            else:
                close_needed += 1                # extra ')'

    return open_needed + close_needed            # total brackets to add
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int minAddToMakeValid(string s) {
    int openNeeded = 0;     // unmatched '(' we're still waiting to close
    int closeNeeded = 0;    // unmatched ')' with nothing to match

    for (char c : s) {
        if (c == '(') {
            openNeeded++;
        } else {
            if (openNeeded > 0) openNeeded--;    // matches an earlier '('
            else closeNeeded++;                  // extra ')'
        }
    }
    return openNeeded + closeNeeded;              // total brackets to add
}
```
</details>

> 🔑 **Why no real stack object here?** We only have ONE type of bracket, so we don't need to check *which* bracket is on top — just *how many* are unmatched. A counter does the job of a stack. (LC 20 needed a real stack because multiple bracket *types* had to match correctly.)

> ⏱️ Time O(n) · Space O(1)

---

## Problem 3 — Minimum Remove to Make Valid Parentheses `(LC 1249)`

> **Teaching line: Instead of checking validity, remove invalid brackets.**
> This is the natural extension of LC 20 — push **indices** of `(` onto the stack. Any `)` with no `(` to match gets marked for removal. Anything left in the stack at the end (unmatched `(`) also gets removed.

```
"lee(t(c)o)de)"  →  "lee(t(c)o)de"
```

**Dry run (tracking indices):**
```
index 3: '(' → push 3          stack: [3]
index 5: '(' → push 5          stack: [3, 5]
index 7: ')' → matches! pop 5   stack: [3]
index 9: ')' → matches! pop 3   stack: []
index 12: ')' → nothing to match → mark index 12 for removal

stack is empty at the end → no leftover '(' to remove
Remove index 12 → "lee(t(c)o)de"  ✅
```

<details>
<summary>☕ Java</summary>

```java
public String minRemoveToMakeValid(String s) {
    Deque<Integer> stack = new ArrayDeque<>();
    Set<Integer> toRemove = new HashSet<>();
    char[] chars = s.toCharArray();

    for (int i = 0; i < chars.length; i++) {
        if (chars[i] == '(') {
            stack.push(i);                       // remember the index
        } else if (chars[i] == ')') {
            if (stack.isEmpty()) toRemove.add(i); // no '(' to match → remove this
            else stack.pop();                     // matched! keep both
        }
    }
    while (!stack.isEmpty()) toRemove.add(stack.pop());  // leftover '(' → remove

    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < chars.length; i++) {
        if (!toRemove.contains(i)) sb.append(chars[i]);
    }
    return sb.toString();
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def minRemoveToMakeValid(self, s):
    stack = []
    to_remove = set()
    chars = list(s)

    for i, c in enumerate(chars):
        if c == '(':
            stack.append(i)                      # remember the index
        elif c == ')':
            if not stack:
                to_remove.add(i)                  # no '(' to match → remove this
            else:
                stack.pop()                       # matched! keep both

    to_remove.update(stack)                       # leftover '(' → remove

    return ''.join(c for i, c in enumerate(chars) if i not in to_remove)
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
string minRemoveToMakeValid(string s) {
    stack<int> st;
    unordered_set<int> toRemove;

    for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(') {
            st.push(i);                          // remember the index
        } else if (s[i] == ')') {
            if (st.empty()) toRemove.insert(i);   // no '(' to match → remove
            else st.pop();                        // matched! keep both
        }
    }
    while (!st.empty()) {                         // leftover '(' → remove
        toRemove.insert(st.top());
        st.pop();
    }

    string result;
    for (int i = 0; i < s.size(); i++) {
        if (toRemove.find(i) == toRemove.end()) result += s[i];
    }
    return result;
}
```
</details>

> 🔑 **The upgrade from LC 20:** instead of pushing the *character*, we push the **index**. That way, when we decide something is invalid, we know exactly *where* to remove it from.

> ⏱️ Time O(n) · Space O(n)

---

## 🧠 End of Day 1 Summary

```
Stack Operations:     Push · Pop · Peek · isEmpty · isFull

Implementation:       Array (fixed size)  vs  Linked List (dynamic size)

Applications:
  LC 20    →  Match brackets using a real stack
  LC 921   →  Count unmatched (no stack object needed — just counters)
  LC 1249  →  Push INDICES, remove what's unmatched
```

> ⭐ **The golden line for the whole day:** *A stack lets you always deal with "the most recent thing first" — perfect for anything about matching pairs or undoing the last action.*

---

# 🟡 DAY 2 — MONOTONIC STACK + ADVANCED APPLICATIONS

> 💡 **Quick revision first:** Stack = LIFO. Push, Pop, Peek, isEmpty. Got it? Good — now let's learn the pattern interviewers love most.

> 🎯 **Two questions to warm up:**
> - Can a **queue** solve the browser back-button? (No — back button needs "most recent page," which is LIFO, not FIFO.)
> - Can a plain **array** find the nearest greater element efficiently? (Only in O(n²) — checking right of every element. Today's trick gets it to O(n).)

## What is a Monotonic Stack?

> 🪜 **Explain Like I'm 5:** A **monotonic stack** is a stack that's always sorted — either always increasing from bottom to top, or always decreasing. Before pushing a new item, you **kick out** anything on top that breaks the order.
>
> This one trick is asked at Amazon, Google, Microsoft, Adobe — it's one of the highest-value patterns you'll learn.

---

## Problem 1 — Next Greater Element (GFG) ⭐⭐

> **Story:** For every element, find the first bigger element to its right.
> `[1, 3, 2, 4]` → `[3, 4, 4, -1]`
> (1's next bigger is 3. 3's next bigger is 4. 2's next bigger is 4. 4 has nothing bigger → -1.)

> **Brute force:** for each element, scan everything to its right. O(n²) — slow.
> **Optimal idea:** walk from the **right side**, keep a **decreasing stack**. While the stack's top is `≤` current element, pop it (it can never be *anyone's* answer once something bigger shows up). Whatever's left on top IS the answer for the current element. Then push current.

**Dry run — `[1, 3, 2, 4]`, walking right to left:**

```
i=3 (val=4): stack empty → ans[3]=-1        push 4        stack: [4]
i=2 (val=2): top=4 > 2, keep it → ans[2]=4  push 2        stack: [4,2]
i=1 (val=3): top=2<=3, pop. top=4>3 → ans[1]=4  push 3    stack: [4,3]
i=0 (val=1): top=3>1 → ans[0]=3             push 1        stack: [4,3,1]

Answer: [3, 4, 4, -1]  ✅
```

<details>
<summary>☕ Java</summary>

```java
static int[] nextGreater(int[] arr) {
    int n = arr.length;
    int[] ans = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() <= arr[i])
            st.pop();                          // can't be anyone's answer anymore
        ans[i] = st.isEmpty() ? -1 : st.peek(); // whatever survives is the answer
        st.push(arr[i]);
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def nextGreater(arr):
    n = len(arr)
    ans = [-1] * n
    st = []

    for i in range(n - 1, -1, -1):
        while st and st[-1] <= arr[i]:
            st.pop()                          # can't be anyone's answer anymore
        if st:
            ans[i] = st[-1]                   # whatever survives is the answer
        st.append(arr[i])
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<int> nextGreater(vector<int>& arr) {
    int n = arr.size();
    vector<int> ans(n);
    stack<int> st;

    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() <= arr[i])
            st.pop();                         // can't be anyone's answer anymore
        ans[i] = st.empty() ? -1 : st.top();   // whatever survives is the answer
        st.push(arr[i]);
    }
    return ans;
}
```
</details>

> 🔑 **Why right-to-left?** Because "next greater" looks *forward*. Walking backward means by the time we reach element `i`, the stack already holds the *processed candidates to its right* — exactly what we need to check.

> ⏱️ Time O(n) — each element is pushed & popped at most once · Space O(n)

---

## Problem 2 — Next Greater Element I `(LC 496)`

> **Pattern: precompute NGE for one array using the stack trick, store answers in a HashMap, then look up answers for a second array.**
> `nums1` is a subset of `nums2` — find each `nums1[i]`'s next greater element *within nums2*.

<details>
<summary>☕ Java</summary>

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    HashMap<Integer, Integer> map = new HashMap<>();
    Stack<Integer> st = new Stack<>();

    for (int i = nums2.length - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() < nums2[i])
            st.pop();
        map.put(nums2[i], st.isEmpty() ? -1 : st.peek());  // store the answer
        st.push(nums2[i]);
    }

    int[] ans = new int[nums1.length];
    for (int i = 0; i < nums1.length; i++)
        ans[i] = map.get(nums1[i]);            // just look it up!
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def nextGreaterElement(self, nums1, nums2):
    mp = {}
    st = []

    for i in range(len(nums2) - 1, -1, -1):
        while st and st[-1] < nums2[i]:
            st.pop()
        mp[nums2[i]] = st[-1] if st else -1     # store the answer
        st.append(nums2[i])

    return [mp[x] for x in nums1]               # just look it up!
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int,int> mp;
    stack<int> st;

    for (int i = nums2.size() - 1; i >= 0; i--) {
        while (!st.empty() && st.top() < nums2[i])
            st.pop();
        mp[nums2[i]] = st.empty() ? -1 : st.top();   // store the answer
        st.push(nums2[i]);
    }

    vector<int> ans;
    for (int x : nums1) ans.push_back(mp[x]);   // just look it up!
    return ans;
}
```
</details>

> 🧠 **This is literally Problem 1 + a HashMap on top.** Once you see the NGE skeleton, this is a two-line addition.

> ⏱️ Time O(n + m) · Space O(n)

---

## Problem 3 — Daily Temperatures `(LC 739)`

> **Story:** For every day, how many days until a warmer temperature?
> `[73,74,75,71,69,72,76,73]` → `[1,1,4,2,1,1,0,0]`
> **Pattern: Next Greater Element, but on INDEX instead of value.**

<details>
<summary>☕ Java</summary>

```java
public int[] dailyTemperatures(int[] temp) {
    int n = temp.length;
    int[] ans = new int[n];
    Stack<Integer> st = new Stack<>();   // stores INDICES this time

    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && temp[st.peek()] <= temp[i])
            st.pop();
        ans[i] = st.isEmpty() ? 0 : st.peek() - i;  // distance, not value
        st.push(i);
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def dailyTemperatures(self, temp):
    n = len(temp)
    ans = [0] * n
    st = []                               # stores INDICES this time

    for i in range(n - 1, -1, -1):
        while st and temp[st[-1]] <= temp[i]:
            st.pop()
        ans[i] = 0 if not st else st[-1] - i  # distance, not value
        st.append(i)
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<int> dailyTemperatures(vector<int>& temp) {
    int n = temp.size();
    vector<int> ans(n);
    stack<int> st;                        // stores INDICES this time

    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && temp[st.top()] <= temp[i])
            st.pop();
        ans[i] = st.empty() ? 0 : st.top() - i;  // distance, not value
        st.push(i);
    }
    return ans;
}
```
</details>

> 🔑 **The upgrade:** push the **index**, not the value. That way you can compute *distance* (`st.peek() - i`) instead of just knowing *what* the next greater value is.

> ⏱️ Time O(n) · Space O(n)

---

## Problem 4 — Online Stock Span `(LC 901)`

> **Story:** Today's stock price arrives. How many consecutive days (ending today) had price `≤` today's price?
> **Pattern: monotonic decreasing stack, but storing (price, span) pairs — and absorbing spans as you pop.**

<details>
<summary>☕ Java</summary>

```java
class StockSpanner {
    Stack<int[]> st;   // each entry: {price, span}

    public StockSpanner() {
        st = new Stack<>();
    }

    public int next(int price) {
        int span = 1;
        while (!st.isEmpty() && st.peek()[0] <= price) {
            span += st.pop()[1];             // absorb the smaller day's span
        }
        st.push(new int[]{price, span});
        return span;
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class StockSpanner:
    def __init__(self):
        self.st = []          # each entry: (price, span)

    def next(self, price):
        span = 1
        while self.st and self.st[-1][0] <= price:
            span += self.st.pop()[1]         # absorb the smaller day's span
        self.st.append((price, span))
        return span
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class StockSpanner {
public:
    stack<pair<int,int>> st;   // each entry: {price, span}

    int next(int price) {
        int span = 1;
        while (!st.empty() && st.top().first <= price) {
            span += st.top().second;         // absorb the smaller day's span
            st.pop();
        }
        st.push({price, span});
        return span;
    }
};
```
</details>

> 🧠 **Why this works:** if today's price beats yesterday's, today "swallows" yesterday's entire span (and everything yesterday had already swallowed) — because all of those days are also `≤` today's price.

> ⏱️ Time O(1) amortized · Space O(n)

---

## Problem 5 — Min Stack `(LC 155)` ⭐

> **Interview question: can we get the minimum element in O(1)?**
> **Answer: keep a SECOND stack that tracks the minimum at every point in time.**

<details>
<summary>☕ Java</summary>

```java
class MinStack {
    Stack<Integer> st;
    Stack<Integer> minSt;

    public MinStack() {
        st = new Stack<>();
        minSt = new Stack<>();
    }

    public void push(int val) {
        st.push(val);
        if (minSt.isEmpty() || val <= minSt.peek())
            minSt.push(val);                 // new minimum (or ties)
    }

    public void pop() {
        if (st.peek().equals(minSt.peek()))
            minSt.pop();                     // the min is leaving too
        st.pop();
    }

    public int top() { return st.peek(); }
    public int getMin() { return minSt.peek(); }   // O(1)!
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class MinStack:
    def __init__(self):
        self.st = []
        self.mn = []

    def push(self, val):
        self.st.append(val)
        if not self.mn or val <= self.mn[-1]:
            self.mn.append(val)              # new minimum (or ties)

    def pop(self):
        if self.st[-1] == self.mn[-1]:
            self.mn.pop()                    # the min is leaving too
        self.st.pop()

    def top(self):
        return self.st[-1]

    def getMin(self):
        return self.mn[-1]                   # O(1)!
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class MinStack {
    stack<int> st;
    stack<int> mn;
public:
    void push(int val) {
        st.push(val);
        if (mn.empty() || val <= mn.top())
            mn.push(val);                    // new minimum (or ties)
    }
    void pop() {
        if (st.top() == mn.top())
            mn.pop();                        // the min is leaving too
        st.pop();
    }
    int top() { return st.top(); }
    int getMin() { return mn.top(); }        // O(1)!
};
```
</details>

> 🔑 **Why `<=` and not `<`?** If you push a duplicate minimum, you need to push it onto `minSt` too — otherwise, when one copy pops, `minSt` would lose track of the other copy still in `st`.

> ⏱️ All operations: Time O(1) · Space O(n)

---

## Problem 6 — Evaluate Reverse Polish Notation `(LC 150)`

> **Postfix math expressions** — operators come *after* their operands.
> `["2","1","+","3","*"]` → `2+1=3`, then `3*3=9` → answer `9`
> **Pattern: number → push. Operator → pop 2, calculate, push the result.**

<details>
<summary>☕ Java</summary>

```java
public int evalRPN(String[] tokens) {
    Stack<Integer> st = new Stack<>();
    for (String s : tokens) {
        if ("+-*/".contains(s) && s.length() == 1) {
            int b = st.pop();                // second operand comes off first
            int a = st.pop();
            switch (s) {
                case "+": st.push(a + b); break;
                case "-": st.push(a - b); break;
                case "*": st.push(a * b); break;
                case "/": st.push(a / b); break;
            }
        } else {
            st.push(Integer.parseInt(s));    // it's a number → push
        }
    }
    return st.peek();
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def evalRPN(self, tokens):
    st = []
    for s in tokens:
        if s in "+-*/" and len(s) == 1:
            b = st.pop()                     # second operand comes off first
            a = st.pop()
            if s == "+": st.append(a + b)
            elif s == "-": st.append(a - b)
            elif s == "*": st.append(a * b)
            else: st.append(int(a / b))
        else:
            st.append(int(s))                # it's a number → push
    return st[-1]
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int evalRPN(vector<string>& tokens) {
    stack<int> st;
    for (string s : tokens) {
        if (s == "+" || s == "-" || s == "*" || s == "/") {
            int b = st.top(); st.pop();      // second operand comes off first
            int a = st.top(); st.pop();
            if (s == "+") st.push(a + b);
            else if (s == "-") st.push(a - b);
            else if (s == "*") st.push(a * b);
            else st.push(a / b);
        } else {
            st.push(stoi(s));                // it's a number → push
        }
    }
    return st.top();
}
```
</details>

> ⚠️ **Order matters:** `b` pops first (it's the *second* operand), then `a`. For `-` and `/`, `a - b` and `a / b` — swapping the order gives the wrong answer.

> ⏱️ Time O(n) · Space O(n)

---

## 🧠 End of Day 2 Summary

```
Monotonic Stack Problems:
  GFG Next Greater Element  →  decreasing stack, pop while top <= current
  LC 496  NGE I              →  same trick + HashMap lookup
  LC 739  Daily Temperatures →  same trick, but stack holds INDICES
  LC 901  Online Stock Span  →  decreasing stack + absorb spans

Design Problems:
  LC 155  Min Stack          →  a second stack tracks the minimum

Expression Evaluation:
  LC 150  Evaluate RPN       →  number push, operator pop-2-calculate-push
```

> 🎓 **Interview patterns learned today:** Monotonic Increasing Stack · Monotonic Decreasing Stack · Index-Based Stack · Pair-Based Stack · Two-Stack Design · Expression Evaluation.
>
> ⭐ These six patterns show up constantly — Amazon, Google, Microsoft, Adobe all love this family. Master these six problems and you'll recognize the pattern instantly on new ones.

---

# 🎯 Practice

**Day 1 — Fundamentals + Bracket Matching:**

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Valid Parentheses | Easy | https://leetcode.com/problems/valid-parentheses/ | Push opening, match & pop on closing |
| Minimum Add to Make Parentheses Valid | Medium | https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/ | Count unmatched (no stack object needed) |
| Minimum Remove to Make Valid Parentheses | Medium | https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/ | Push indices, remove what's unmatched |

**Day 2 — Monotonic Stack + Advanced:**

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Next Greater Element | Medium | https://www.geeksforgeeks.org/problems/next-larger-element-1587115620/1 | Decreasing stack, right to left |
| Next Greater Element I | Easy | https://leetcode.com/problems/next-greater-element-i/ | NGE trick + HashMap lookup |
| Daily Temperatures | Medium | https://leetcode.com/problems/daily-temperatures/ | NGE trick, stack holds indices |
| Online Stock Span | Medium | https://leetcode.com/problems/online-stock-span/ | Decreasing stack + absorb spans |
| Min Stack | Medium | https://leetcode.com/problems/min-stack/ | Second stack tracks the minimum |
| Evaluate Reverse Polish Notation | Medium | https://leetcode.com/problems/evaluate-reverse-polish-notation/ | Number→push, operator→pop 2, calculate |

**Coming next (Day 3 preview):** Largest Rectangle in Histogram (LC 84), Asteroid Collision (LC 735), Decode String (LC 394) — all build on today's monotonic stack pattern.

> 💡 **Do Day 1 first, then Day 2 top to bottom.** The monotonic stack trick (Problem 1) is the engine behind Problems 2, 3, and 4 — nail that one and the rest fall into place. I'm right here for any doubt. 💪 — *Ajai Raj (Mentor)*
