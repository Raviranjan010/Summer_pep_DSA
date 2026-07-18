# 🐍 Python Syntax for DSA — Your Cheat Sheet

> Keep this open while solving. Python is the shortest of the three languages — this covers everything you need for arrays, strings, dicts, sets, heaps, and binary search.

---

## 1. The Basics (no boilerplate needed!)

```python
print("Hello")              # print with new line
print(a, b)                 # print multiple values
x = int(input())            # read one integer
words = input().split()     # read a line, split into words
```

> 🔑 Python needs **no** class structure or `main` function for basic execution.

---

## 2. Variables (dynamic typing)

```python
a = 5                       # int
d = 3.14                    # float
c = 'a'                     # Python has no separate char type — it's a 1-length string
flag = True                 # note the capital T (and False)
s = "hello"                 # string
```

> ⚠️ `True`, `False`, and `None` are capitalized in Python.

---

## 3. Lists (Python's array)

```python
arr = [10, 20, 30]
arr = [0] * 5               # five zeros: [0,0,0,0,0]
n = len(arr)                # size
arr[0] = 99                 # set
x = arr[2]                  # read

arr.append(40)              # append to end
arr.pop()                   # remove & return last from end
arr.sort()                  # sort ascending (in-place)
arr.sort(reverse=True)      # sort descending (in-place)
arr.reverse()               # reverse in-place
total = sum(arr)            # sum of all elements
biggest = max(arr)          # largest
smallest = min(arr)         # smallest

# 2D list (grid)
grid = [[0] * 4 for _ in range(3)]   # 3 rows, 4 cols
grid[1][2] = 7
```

> ⚠️ **Careful:** `[[0]*4]*3` is a trap — it makes 3 copies pointing to the *same* row object. Modify one cell, and the whole column changes! Always use `[[0]*4 for _ in range(3)]`.

---

## 4. Loops

```python
for i in range(n):              # 0 to n-1
    pass
for i in range(1, n):           # 1 to n-1
    pass
for i in range(n-1, -1, -1):    # backwards: n-1 down to 0
    pass
for x in arr:                   # for-each loop
    pass
for i, x in enumerate(arr):     # index AND value together
    pass
while condition:
    pass
```

---

## 5. Slicing (Python's superpower)

```python
s[1:4]        # chars from index 1 to 3 (4 excluded)
s[:3]         # first 3 chars
s[2:]         # from index 2 to end
s[::-1]       # REVERSED (whole thing backwards)
arr[::-1]     # reversed list
```

---

## 6. Strings

```python
s = "hello"
len(s)                      # length
s[2]                        # character at index 2
s.upper()   s.lower()
s.split()                   # "a b c" → ['a','b','c']
"".join(list_of_chars)      # ['a','b'] → "ab"
s == "hello"                # compares contents directly
'ell' in s                  # substring check → True
s.isalnum()                 # letters/numbers only? (bool)
list(s)                     # string → list of characters ['h','e','l','l','o']
```

---

## 7. The `char` ↔ `int` Trick (crucial for strings)

```python
pos = ord(c) - ord('a')     # 'a'→0, 'b'→1, ... 'z'→25
back = chr(ord('a') + pos)  # 0→'a', 1→'b', ...
digit = int(c)              # '7' → 7

# 26-size frequency list
freq = [0] * 26
freq[ord(c) - ord('a')] += 1
```

---

## 8. Dictionaries (HashMap equivalent)

```python
mp = {}

mp['a'] = 1                 # add / overwrite
mp['a']                     # read (Throws KeyError if key is missing!)
'a' in mp                   # check key existence
del mp['a']                 # delete key
len(mp)

# ⭐ getOrDefault equivalent:
mp[c] = mp.get(c, 0) + 1    # "get c, or 0 if missing, then +1"

# loops
for key in mp:              # loop over keys
    pass
for key, val in mp.items(): # loop over keys AND values
    pass
```

### collections.Counter (Auto-counting dict)
```python
from collections import Counter

freq = Counter("hello")     # {'l': 2, 'h': 1, 'e': 1, 'o': 1}
freq['l']                   # 2
freq.most_common()          # [('l', 2), ('h', 1), ...] (sorted by counts descending)
```

---

## 9. Sets (Uniqueness check)

```python
seen = set()

seen.add(5)                 # add
5 in seen                   # seen it? (O(1) average check)
seen.remove(5)
len(seen)

# quick duplicate check:
has_duplicates = len(set(arr)) != len(arr)
```

---

## 10. Advanced Libraries for DSA

### A. heapq (PriorityQueue / Heaps)
Python's `heapq` module provides a Min-Heap implementation on standard lists in-place.
```python
import heapq

heap = []
# 1. Min Heap
heapq.heappush(heap, 10)
heapq.heappush(heap, 5)
heap[0]                     # Peek: returns 5 (smallest)
heapq.heappop(heap)         # Pop: removes and returns 5

# 2. Max Heap: Multiply values by -1 before pushing
max_heap = []
heapq.heappush(max_heap, -10)
heapq.heappush(max_heap, -15)
largest = -heapq.heappop(max_heap) # Returns 15
```

### B. bisect (Binary Search Library)
Provides fast binary search functions on sorted lists.
```python
import bisect

arr = [1, 3, 3, 5, 7]
# 1. bisect_left: Find index of first element >= target (lower bound)
idx = bisect.bisect_left(arr, 3)    # Returns 1 (index of first '3')

# 2. bisect_right: Find index of first element > target (upper bound)
idx = bisect.bisect_right(arr, 3)   # Returns 3 (index of '5')
```

### C. collections.deque (Double-Ended Queue)
Use for stacks and queues. Fast $O(1)$ insertions/deletions at both ends.
```python
from collections import deque

dq = deque()
dq.append(10)               # Append at back
dq.appendleft(20)           # Insert at front
dq.pop()                    # Remove from back
dq.popleft()                # Remove from front
```

---

## 11. Time Complexities Python Cheat-Sheet

| Structure | Operation | Syntax | Time Complexity |
| :--- | :--- | :--- | :--- |
| **List** | Access / Append / Pop | `arr[i]` / `append` / `pop()` | $O(1)$ |
| | Insert / Delete (middle) | `insert(idx, x)` / `pop(idx)` | $O(n)$ |
| | List Slice | `arr[i:j]` | $O(j - i)$ |
| **Dict / Set** | Insert / Lookup / Delete | `d[k]` / `k in d` / `del d[k]` | $O(1)$ average |
| **Heap** | Push / Pop | `heappush` / `heappop` | $O(\log n)$ |
| **Deque** | Append / Pop at ends | `appendleft` / `popleft` | $O(1)$ |

---

## 12. Sorting with Custom Order

```python
arr.sort()                          # ascending (in-place)
arr.sort(reverse=True)              # descending
sorted_arr = sorted(arr)            # returns a new sorted list

# sort using a lambda key function
words = ["apple", "banana", "kiwi"]
words.sort(key=len)                 # sort by length: ["kiwi", "apple", "banana"]
words.sort(key=lambda w: -len(w))   # sort by length descending
```
