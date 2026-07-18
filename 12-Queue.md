# 🚶 12 — Queue

> 🎟️ **Explain Like I'm 5:** A ticket line at a movie counter. Whoever joined **first** gets served **first**. New people join at the **back**. That's a Queue — **FIFO: First In, First Out.** The opposite of a stack!

```
Front                          Back
 ↓                               ↓
[10] → [20] → [30] → [40]
(served next)          (joined most recently)
```

| | Stack | Queue |
|---|-------|-------|
| Order | LIFO — last in, first out | FIFO — first in, first out |
| Real-world example | Stack of plates | Ticket line |
| Add | Push (top) | Enqueue / offer (back) |
| Remove | Pop (top) | Dequeue / poll (front) |

---

## The 4 Core Operations

| Operation | What it does |
|-----------|-------------|
| **Enqueue / Push** | Add to the **back** |
| **Dequeue / Pop** | Remove from the **front** |
| **Peek / Front** | Look at the front item without removing |
| **isEmpty** | Check if the queue has nothing in it |

---

# 🟢 DAY 1 — QUEUE BASICS + APPLICATIONS

## Implementation 1 — Queue Using Array

> **Two pointers: `front` and `rear`.** Enqueue adds at `rear` and moves it forward. Dequeue reads `front` and moves it forward.

<details>
<summary>☕ Java</summary>

```java
class MyQueue {
    int front, rear;
    int[] arr = new int[100005];

    MyQueue() {
        front = 0;
        rear = 0;
    }

    void push(int x) {
        arr[rear++] = x;              // insert at rear, then move rear forward
    }

    int pop() {
        if (front == rear)            // nothing between front and rear = empty
            return -1;
        return arr[front++];          // read front, then move front forward
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class MyQueue:
    def __init__(self):
        self.arr = []
        self.front = 0

    def push(self, x):
        self.arr.append(x)            # insert at the back

    def pop(self):
        if self.front == len(self.arr):   # nothing left = empty
            return -1
        val = self.arr[self.front]
        self.front += 1                # move front forward
        return val
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class MyQueue {
public:
    int front = 0, rear = 0;
    int arr[100005];

    void push(int x) {
        arr[rear++] = x;               // insert at rear, then move rear forward
    }

    int pop() {
        if (front == rear)             // nothing between = empty
            return -1;
        return arr[front++];           // read front, then move front forward
    }
};
```
</details>

> ⏱️ Time O(1) · Space O(1) per operation (array is preallocated)

---

## Implementation 2 — Queue Using Linked List

> **Two pointers into the same list: `front` and `rear`.** Enqueue attaches a new node at `rear`. Dequeue removes from `front`. No size limit.

<details>
<summary>☕ Java</summary>

```java
class QueueNode {
    int data;
    QueueNode next;
    QueueNode(int a) { data = a; next = null; }
}

class MyQueue {
    QueueNode front, rear;

    void push(int a) {
        QueueNode newNode = new QueueNode(a);
        if (rear == null) {              // empty queue — first node
            front = rear = newNode;
            return;
        }
        rear.next = newNode;              // attach after current rear
        rear = newNode;                   // new node becomes rear
    }

    int pop() {
        if (front == null) return -1;
        int val = front.data;
        front = front.next;               // front moves forward
        if (front == null) rear = null;   // queue is now empty — reset rear too
        return val;
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class QueueNode:
    def __init__(self, a):
        self.data = a
        self.next = None

class MyQueue:
    def __init__(self):
        self.front = None
        self.rear = None

    def push(self, a):
        new_node = QueueNode(a)
        if self.rear is None:             # empty queue — first node
            self.front = self.rear = new_node
            return
        self.rear.next = new_node         # attach after current rear
        self.rear = new_node              # new node becomes rear

    def pop(self):
        if self.front is None:
            return -1
        val = self.front.data
        self.front = self.front.next      # front moves forward
        if self.front is None:
            self.rear = None              # queue is now empty — reset rear
        return val
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
struct QueueNode {
    int data;
    QueueNode* next;
    QueueNode(int a) { data = a; next = nullptr; }
};

class MyQueue {
public:
    QueueNode* front = nullptr;
    QueueNode* rear = nullptr;

    void push(int a) {
        QueueNode* newNode = new QueueNode(a);
        if (rear == nullptr) {           // empty queue — first node
            front = rear = newNode;
            return;
        }
        rear->next = newNode;             // attach after current rear
        rear = newNode;                   // new node becomes rear
    }

    int pop() {
        if (front == nullptr) return -1;
        int val = front->data;
        front = front->next;              // front moves forward
        if (front == nullptr) rear = nullptr;  // reset rear if now empty
        return val;
    }
};
```
</details>

> ⏱️ Time O(1) · Space O(n)

---

## In Real Code, Use the Built-In Queue

<details>
<summary>☕ Java</summary>

```java
Queue<Integer> q = new LinkedList<>();
q.offer(10);      // enqueue
q.poll();         // dequeue
q.peek();         // front
q.isEmpty();
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque
q = deque()
q.append(10)       # enqueue
q.popleft()         # dequeue
q[0]                 # front
len(q) == 0            # isEmpty
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
queue<int> q;
q.push(10);        // enqueue
q.pop();            // dequeue
q.front();           // front
q.empty();
```
</details>

> ⚠️ **Python tip:** never use a plain `list` as a queue — `list.pop(0)` is O(n). Always use `collections.deque` for O(1) dequeue.

---

## Application 1 — Number of Recent Calls `(LC 933)`

> **Story:** Track ping timestamps. For each new ping at time `t`, count how many pings happened in `[t-3000, t]`.
> **Pattern: enqueue every ping, then dequeue anything too old from the front.**

<details>
<summary>☕ Java</summary>

```java
class RecentCounter {
    Queue<Integer> q;

    public RecentCounter() {
        q = new LinkedList<>();
    }

    public int ping(int t) {
        q.offer(t);
        while (!q.isEmpty() && q.peek() < t - 3000)
            q.poll();                    // too old — remove from front
        return q.size();
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

class RecentCounter:
    def __init__(self):
        self.q = deque()

    def ping(self, t):
        self.q.append(t)
        while self.q and self.q[0] < t - 3000:
            self.q.popleft()             # too old — remove from front
        return len(self.q)
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class RecentCounter {
public:
    queue<int> q;

    int ping(int t) {
        q.push(t);
        while (!q.empty() && q.front() < t - 3000)
            q.pop();                     // too old — remove from front
        return q.size();
    }
};
```
</details>

> ⏱️ Time O(1) amortized · Space O(n)

---

## Application 2 — Moving Average from Data Stream `(LC 346)`

> **Keep only the last `size` values and return their average.** Enqueue new values; once the queue exceeds `size`, dequeue the oldest.

<details>
<summary>☕ Java</summary>

```java
class MovingAverage {
    Queue<Integer> q;
    int size;
    double sum;

    public MovingAverage(int size) {
        this.size = size;
        q = new LinkedList<>();
        sum = 0;
    }

    public double next(int val) {
        q.offer(val);
        sum += val;
        if (q.size() > size) {
            sum -= q.poll();              // drop the oldest value
        }
        return sum / q.size();
    }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

class MovingAverage:
    def __init__(self, size):
        self.size = size
        self.q = deque()
        self.total = 0

    def next(self, val):
        self.q.append(val)
        self.total += val
        if len(self.q) > self.size:
            self.total -= self.q.popleft()  # drop the oldest value
        return self.total / len(self.q)
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class MovingAverage {
public:
    queue<int> q;
    int size;
    double sum = 0;

    MovingAverage(int size) : size(size) {}

    double next(int val) {
        q.push(val);
        sum += val;
        if (q.size() > size) {
            sum -= q.front();             // drop the oldest value
            q.pop();
        }
        return sum / q.size();
    }
};
```
</details>

> ⏱️ Time O(1) · Space O(k)

---

## Application 3 — Number of Students Unable to Eat Lunch `(LC 1700)`

> **Story:** Students queue for sandwiches (stack of sandwiches on top). If the front student doesn't like the top sandwich, they go to the **back** of the line. Find how many never get to eat.
> **Trick: if a full lap of the queue passes with no match, everyone left is stuck forever — stop.**

<details>
<summary>☕ Java</summary>

```java
public int countStudents(int[] students, int[] sandwiches) {
    Queue<Integer> q = new LinkedList<>();
    for (int s : students) q.offer(s);

    int index = 0, count = 0;

    while (!q.isEmpty() && count < q.size()) {
        if (q.peek() == sandwiches[index]) {
            q.poll();                     // this student eats!
            index++;
            count = 0;                    // reset — progress was made
        } else {
            q.offer(q.poll());            // move to the back of the line
            count++;                      // track how many skipped in a row
        }
    }
    return q.size();                      // stuck students
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

def countStudents(self, students, sandwiches):
    q = deque(students)
    index = 0
    count = 0

    while q and count < len(q):
        if q[0] == sandwiches[index]:
            q.popleft()                   # this student eats!
            index += 1
            count = 0                     # reset — progress was made
        else:
            q.append(q.popleft())         # move to the back of the line
            count += 1                    # track how many skipped in a row
    return len(q)                         # stuck students
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int countStudents(vector<int>& students, vector<int>& sandwiches) {
    queue<int> q;
    for (int s : students) q.push(s);

    int index = 0, count = 0;

    while (!q.empty() && count < q.size()) {
        if (q.front() == sandwiches[index]) {
            q.pop();                      // this student eats!
            index++;
            count = 0;                    // reset — progress was made
        } else {
            q.push(q.front()); q.pop();   // move to the back of the line
            count++;                      // track how many skipped in a row
        }
    }
    return q.size();                      // stuck students
}
```
</details>

> ⏱️ Time O(n²) worst case · Space O(n)

---

## Application 4 — Dota2 Senate `(LC 649)`

> **Two queues, one per team.** Each round, compare the earliest R and earliest D. Whoever comes first "bans" the other (removes them), and rejoins the queue at the very end (`+n` simulates going to the next round).

<details>
<summary>☕ Java</summary>

```java
public String predictPartyVictory(String senate) {
    Queue<Integer> radiant = new LinkedList<>();
    Queue<Integer> dire = new LinkedList<>();
    int n = senate.length();

    for (int i = 0; i < n; i++) {
        if (senate.charAt(i) == 'R') radiant.offer(i);
        else dire.offer(i);
    }

    while (!radiant.isEmpty() && !dire.isEmpty()) {
        int r = radiant.poll();
        int d = dire.poll();
        if (r < d) {
            radiant.offer(r + n);         // R bans D, rejoins next round
        } else {
            dire.offer(d + n);            // D bans R, rejoins next round
        }
    }
    return radiant.isEmpty() ? "Dire" : "Radiant";
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

def predictPartyVictory(self, senate):
    radiant, dire = deque(), deque()
    n = len(senate)

    for i, c in enumerate(senate):
        (radiant if c == 'R' else dire).append(i)

    while radiant and dire:
        r = radiant.popleft()
        d = dire.popleft()
        if r < d:
            radiant.append(r + n)          # R bans D, rejoins next round
        else:
            dire.append(d + n)             # D bans R, rejoins next round

    return "Radiant" if radiant else "Dire"
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
string predictPartyVictory(string senate) {
    queue<int> radiant, dire;
    int n = senate.size();

    for (int i = 0; i < n; i++) {
        if (senate[i] == 'R') radiant.push(i);
        else dire.push(i);
    }

    while (!radiant.empty() && !dire.empty()) {
        int r = radiant.front(); radiant.pop();
        int d = dire.front(); dire.pop();
        if (r < d) radiant.push(r + n);    // R bans D, rejoins next round
        else dire.push(d + n);             // D bans R, rejoins next round
    }
    return radiant.empty() ? "Dire" : "Radiant";
}
```
</details>

> 🔑 **Why `+ n`?** Adding `n` pushes the senator's "position" past everyone currently in this round, simulating them rejoining at the back for the *next* round while preserving relative order.

> ⏱️ Time O(n) · Space O(n)

---

## 🧠 Day 1 Summary

```
Queue Using Array        →  front/rear pointers into a fixed array
Queue Using Linked List   →  front/rear pointers into nodes, dynamic size
Recent Counter            →  enqueue + dequeue anything too old
Moving Average             →  enqueue + dequeue when size exceeds k
Students Unable to Eat     →  rotate to back, stop after a full stuck lap
Dota2 Senate                →  two queues, ban + rejoin with +n
```

---

# 🟡 DAY 2 — ADVANCED QUEUE / BFS PROBLEMS

## Application 5 — Implement Stack Using Queues `(LC 225)`

> **Can a queue (FIFO) act like a stack (LIFO)?** Yes — after pushing a new element, rotate the queue so the newest element ends up at the **front**.

<details>
<summary>☕ Java</summary>

```java
class MyStack {
    Queue<Integer> q;

    public MyStack() { q = new LinkedList<>(); }

    public void push(int x) {
        q.offer(x);
        for (int i = 0; i < q.size() - 1; i++)
            q.offer(q.poll());             // rotate everyone before x to the back
    }

    public int pop() { return q.poll(); }
    public int top() { return q.peek(); }
    public boolean empty() { return q.isEmpty(); }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.q = deque()

    def push(self, x):
        self.q.append(x)
        for _ in range(len(self.q) - 1):
            self.q.append(self.q.popleft())   # rotate everyone before x to back

    def pop(self):
        return self.q.popleft()

    def top(self):
        return self.q[0]

    def empty(self):
        return len(self.q) == 0
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class MyStack {
public:
    queue<int> q;

    void push(int x) {
        q.push(x);
        for (int i = 0; i < (int)q.size() - 1; i++) {
            q.push(q.front());              // rotate everyone before x to back
            q.pop();
        }
    }

    int pop() { int v = q.front(); q.pop(); return v; }
    int top() { return q.front(); }
    bool empty() { return q.empty(); }
};
```
</details>

> ⏱️ Push: O(n) · Pop/Top: O(1) · Space O(n)

---

## Application 6 — Implement Queue Using Stacks `(LC 232)`

> **The reverse trick: can two stacks (LIFO) act like a queue (FIFO)?** Yes — one stack for incoming pushes, one for outgoing pops. Only flip between them when the "outgoing" stack runs dry.

<details>
<summary>☕ Java</summary>

```java
class MyQueue {
    Stack<Integer> s1, s2;   // s1 = incoming, s2 = outgoing

    public MyQueue() { s1 = new Stack<>(); s2 = new Stack<>(); }

    public void push(int x) { s1.push(x); }

    public int pop() {
        peek();                              // makes sure s2 has data
        return s2.pop();
    }

    public int peek() {
        if (s2.isEmpty()) {
            while (!s1.isEmpty())
                s2.push(s1.pop());           // reverse s1 into s2
        }
        return s2.peek();
    }

    public boolean empty() { return s1.isEmpty() && s2.isEmpty(); }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class MyQueue:
    def __init__(self):
        self.s1 = []          # incoming
        self.s2 = []          # outgoing

    def push(self, x):
        self.s1.append(x)

    def pop(self):
        self.peek()            # makes sure s2 has data
        return self.s2.pop()

    def peek(self):
        if not self.s2:
            while self.s1:
                self.s2.append(self.s1.pop())   # reverse s1 into s2
        return self.s2[-1]

    def empty(self):
        return not self.s1 and not self.s2
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class MyQueue {
public:
    stack<int> s1, s2;   // s1 = incoming, s2 = outgoing

    void push(int x) { s1.push(x); }

    int pop() {
        peek();                              // makes sure s2 has data
        int v = s2.top(); s2.pop();
        return v;
    }

    int peek() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();                     // reverse s1 into s2
            }
        }
        return s2.top();
    }

    bool empty() { return s1.empty() && s2.empty(); }
};
```
</details>

> 🧠 **Why does reversing a stack give queue order?** Pushing 1,2,3 into `s1` puts 3 on top. Popping all of `s1` into `s2` reverses that — 1 ends up on top of `s2`. So `s2` naturally pops in the original (FIFO) order!

> ⏱️ Time O(1) amortized · Space O(n)

---

## Application 7 — Sliding Window Maximum `(LC 239)` ⭐⭐

> **The hardest — and most important — queue pattern. A "Deque" (double-ended queue) that stays monotonically DECREASING.**
> **Idea:** for every window, we only care about the *maximum*. Keep a deque of *indices* where values are decreasing front-to-back. The front is always the current window's max.

**The 3 rules for each new element:**
1. Remove indices from the **front** that have fallen out of the window (`index <= i - k`).
2. Remove indices from the **back** whose value is `≤` the current value (they can never be the max again).
3. Add the current index to the back. If the window is full, the front is your answer.

<details>
<summary>☕ Java</summary>

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> dq = new LinkedList<>();   // stores INDICES
    int n = nums.length;
    int[] ans = new int[n - k + 1];
    int idx = 0;

    for (int i = 0; i < n; i++) {
        while (!dq.isEmpty() && dq.peekFirst() <= i - k)
            dq.pollFirst();                    // rule 1: out of window

        while (!dq.isEmpty() && nums[dq.peekLast()] <= nums[i])
            dq.pollLast();                      // rule 2: can't be the max anymore

        dq.offerLast(i);                        // rule 3: add current

        if (i >= k - 1)
            ans[idx++] = nums[dq.peekFirst()];  // front = current max
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

def maxSlidingWindow(self, nums, k):
    dq = deque()          # stores INDICES
    n = len(nums)
    ans = []

    for i in range(n):
        while dq and dq[0] <= i - k:
            dq.popleft()                        # rule 1: out of window

        while dq and nums[dq[-1]] <= nums[i]:
            dq.pop()                             # rule 2: can't be the max anymore

        dq.append(i)                             # rule 3: add current

        if i >= k - 1:
            ans.append(nums[dq[0]])              # front = current max

    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq;         // stores INDICES
    int n = nums.size();
    vector<int> ans;

    for (int i = 0; i < n; i++) {
        while (!dq.empty() && dq.front() <= i - k)
            dq.pop_front();                     // rule 1: out of window

        while (!dq.empty() && nums[dq.back()] <= nums[i])
            dq.pop_back();                        // rule 2: can't be the max anymore

        dq.push_back(i);                          // rule 3: add current

        if (i >= k - 1)
            ans.push_back(nums[dq.front()]);      // front = current max
    }
    return ans;
}
```
</details>

> 🔑 **This is a Monotonic Queue** — the queue version of the monotonic stack you learned in Stack Day 2! Same "kick out anything that can't win" idea, just applied to a sliding window.

> ⏱️ Time O(n) — each index pushed & popped at most once · Space O(k)

---

## Application 8 — Rotting Oranges `(LC 994)` — Multi-Source BFS

> **All rotten oranges start in the queue AT ONCE.** Each round ("minute"), every rotten orange spreads to its fresh neighbors simultaneously. This is **multi-source BFS**.

<details>
<summary>☕ Java</summary>

```java
public int orangesRotting(int[][] grid) {
    Queue<int[]> q = new LinkedList<>();
    int fresh = 0, time = 0;
    int rows = grid.length, cols = grid[0].length;

    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == 2) q.offer(new int[]{i, j});   // ALL rotten oranges start together
            if (grid[i][j] == 1) fresh++;
        }

    int[][] dir = {{1,0},{-1,0},{0,1},{0,-1}};

    while (!q.isEmpty() && fresh > 0) {
        int size = q.size();
        for (int i = 0; i < size; i++) {          // process one full "minute" at a time
            int[] curr = q.poll();
            for (int[] d : dir) {
                int nr = curr[0] + d[0], nc = curr[1] + d[1];
                if (nr >= 0 && nc >= 0 && nr < rows && nc < cols && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    fresh--;
                    q.offer(new int[]{nr, nc});
                }
            }
        }
        time++;
    }
    return fresh == 0 ? time : -1;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

def orangesRotting(self, grid):
    q = deque()
    fresh = 0
    rows, cols = len(grid), len(grid[0])

    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 2:
                q.append((i, j))              # ALL rotten oranges start together
            elif grid[i][j] == 1:
                fresh += 1

    directions = [(1,0),(-1,0),(0,1),(0,-1)]
    time = 0

    while q and fresh > 0:
        for _ in range(len(q)):                # process one full "minute" at a time
            r, c = q.popleft()
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                    grid[nr][nc] = 2
                    fresh -= 1
                    q.append((nr, nc))
        time += 1

    return time if fresh == 0 else -1
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int orangesRotting(vector<vector<int>>& grid) {
    queue<pair<int,int>> q;
    int fresh = 0, time = 0;
    int rows = grid.size(), cols = grid[0].size();

    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == 2) q.push({i, j});   // ALL rotten oranges start together
            if (grid[i][j] == 1) fresh++;
        }

    vector<pair<int,int>> dir = {{1,0},{-1,0},{0,1},{0,-1}};

    while (!q.empty() && fresh > 0) {
        int size = q.size();
        for (int i = 0; i < size; i++) {           // process one full "minute" at a time
            auto [r, c] = q.front(); q.pop();
            for (auto [dr, dc] : dir) {
                int nr = r + dr, nc = c + dc;
                if (nr >= 0 && nc >= 0 && nr < rows && nc < cols && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    fresh--;
                    q.push({nr, nc});
                }
            }
        }
        time++;
    }
    return fresh == 0 ? time : -1;
}
```
</details>

> 🔑 **The `size = q.size()` trick** processes exactly one "layer" (one minute) at a time — this is the standard BFS pattern for level-by-level or time-step problems.

> ⏱️ Time O(rows × cols) · Space O(rows × cols)

---

## Application 9 — Binary Tree Level Order Traversal `(LC 102)` — Tree BFS

> **Visit a tree level by level, left to right.** Same `size = q.size()` trick as Rotting Oranges — but for tree levels instead of grid minutes.

<details>
<summary>☕ Java</summary>

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int size = q.size();                  // exactly one level's worth
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = q.poll();
            level.add(node.val);
            if (node.left != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
        ans.add(level);
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import deque

def levelOrder(self, root):
    ans = []
    if not root:
        return ans

    q = deque([root])
    while q:
        size = len(q)                          # exactly one level's worth
        level = []
        for _ in range(size):
            node = q.popleft()
            level.append(node.val)
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        ans.append(level)
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> ans;
    if (!root) return ans;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int size = q.size();                   // exactly one level's worth
        vector<int> level;
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        ans.push_back(level);
    }
    return ans;
}
```
</details>

> ⏱️ Time O(n) · Space O(n)

---

## Application 10 — Design Circular Deque `(LC 641)` — Design Question

> **A deque (double-ended queue) with a fixed-size circular array.** Insert/delete from BOTH front and back. The circular trick: use `% capacity` so indices wrap around instead of running off the array.

<details>
<summary>☕ Java</summary>

```java
class MyCircularDeque {
    int[] deque;
    int front, rear, size, capacity;

    public MyCircularDeque(int k) {
        capacity = k;
        deque = new int[k];
        front = 0;
        rear = k - 1;
        size = 0;
    }

    public boolean insertFront(int value) {
        if (isFull()) return false;
        front = (front - 1 + capacity) % capacity;   // wrap backward
        deque[front] = value;
        size++;
        return true;
    }

    public boolean insertLast(int value) {
        if (isFull()) return false;
        rear = (rear + 1) % capacity;                  // wrap forward
        deque[rear] = value;
        size++;
        return true;
    }

    public boolean deleteFront() {
        if (isEmpty()) return false;
        front = (front + 1) % capacity;
        size--;
        return true;
    }

    public boolean deleteLast() {
        if (isEmpty()) return false;
        rear = (rear - 1 + capacity) % capacity;
        size--;
        return true;
    }

    public int getFront() { return isEmpty() ? -1 : deque[front]; }
    public int getRear() { return isEmpty() ? -1 : deque[rear]; }
    public boolean isEmpty() { return size == 0; }
    public boolean isFull() { return size == capacity; }
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class MyCircularDeque:
    def __init__(self, k):
        self.capacity = k
        self.deque = [0] * k
        self.front = 0
        self.rear = k - 1
        self.size = 0

    def insertFront(self, value):
        if self.isFull(): return False
        self.front = (self.front - 1 + self.capacity) % self.capacity  # wrap backward
        self.deque[self.front] = value
        self.size += 1
        return True

    def insertLast(self, value):
        if self.isFull(): return False
        self.rear = (self.rear + 1) % self.capacity      # wrap forward
        self.deque[self.rear] = value
        self.size += 1
        return True

    def deleteFront(self):
        if self.isEmpty(): return False
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return True

    def deleteLast(self):
        if self.isEmpty(): return False
        self.rear = (self.rear - 1 + self.capacity) % self.capacity
        self.size -= 1
        return True

    def getFront(self): return -1 if self.isEmpty() else self.deque[self.front]
    def getRear(self): return -1 if self.isEmpty() else self.deque[self.rear]
    def isEmpty(self): return self.size == 0
    def isFull(self): return self.size == self.capacity
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
class MyCircularDeque {
public:
    vector<int> deque;
    int front, rear, size, capacity;

    MyCircularDeque(int k) {
        capacity = k;
        deque.resize(k);
        front = 0;
        rear = k - 1;
        size = 0;
    }

    bool insertFront(int value) {
        if (isFull()) return false;
        front = (front - 1 + capacity) % capacity;   // wrap backward
        deque[front] = value;
        size++;
        return true;
    }

    bool insertLast(int value) {
        if (isFull()) return false;
        rear = (rear + 1) % capacity;                  // wrap forward
        deque[rear] = value;
        size++;
        return true;
    }

    bool deleteFront() {
        if (isEmpty()) return false;
        front = (front + 1) % capacity;
        size--;
        return true;
    }

    bool deleteLast() {
        if (isEmpty()) return false;
        rear = (rear - 1 + capacity) % capacity;
        size--;
        return true;
    }

    int getFront() { return isEmpty() ? -1 : deque[front]; }
    int getRear() { return isEmpty() ? -1 : deque[rear]; }
    bool isEmpty() { return size == 0; }
    bool isFull() { return size == capacity; }
};
```
</details>

> 🔑 **Why `+ capacity` before `% capacity`?** `front - 1` can go negative (e.g. `0 - 1 = -1`). Adding `capacity` first guarantees a non-negative number before wrapping, so `-1 % capacity` never happens.

> ⏱️ All operations: Time O(1) · Space O(k)

---

## 🧠 Day 2 Summary

```
LC 225   Stack Using Queue     →  push rotates new element to the front
LC 232   Queue Using Stack     →  two stacks, flip when outgoing is empty
LC 239   Sliding Window Max    →  Monotonic Deque (decreasing, holds indices)
LC 994   Rotting Oranges       →  Multi-source BFS, size=q.size() per minute
LC 102   Level Order Traversal →  Tree BFS, size=q.size() per level
LC 641   Circular Deque        →  fixed array + modulo wraparound
```

> 🎓 **The big picture:** Day 1 built comfort with plain queues. Day 2 showed queues **combined** with other structures — stacks (225, 232), monotonic tricks (239), and BFS (994, 102). That's the real interview skill: recognizing when "process things in order, level by level" means **queue**.

---

# 🎯 Practice

**Day 1 — Queue Basics + Applications:**

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Number of Recent Calls | Easy | https://leetcode.com/problems/number-of-recent-calls/ | Enqueue + dequeue anything too old |
| Moving Average from Data Stream | Easy | https://leetcode.com/problems/moving-average-from-data-stream/ | Enqueue + dequeue when size > k |
| Number of Students Unable to Eat Lunch | Easy | https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/ | Rotate to back, stop after a stuck lap |
| Dota2 Senate | Medium | https://leetcode.com/problems/dota2-senate/ | Two queues, ban + rejoin with +n |

**Day 2 — Advanced Queue / BFS:**

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Implement Stack Using Queues | Easy | https://leetcode.com/problems/implement-stack-using-queues/ | Rotate after every push |
| Implement Queue Using Stacks | Easy | https://leetcode.com/problems/implement-queue-using-stacks/ | Two stacks, flip on demand |
| Sliding Window Maximum | Hard | https://leetcode.com/problems/sliding-window-maximum/ | Monotonic decreasing deque of indices |
| Rotting Oranges | Medium | https://leetcode.com/problems/rotting-oranges/ | Multi-source BFS |
| Binary Tree Level Order Traversal | Medium | https://leetcode.com/problems/binary-tree-level-order-traversal/ | Tree BFS, level by level |
| Design Circular Deque | Medium | https://leetcode.com/problems/design-circular-deque/ | Circular array + modulo |

> 💡 **Do Day 1 top to bottom first — get comfortable with plain queue mechanics.** Day 2 is where queues start combining with other structures (stacks, BFS, monotonic tricks) — the real interview-level skill. I'm right here for any doubt. 💪 — *Ajai Raj (Mentor)*
