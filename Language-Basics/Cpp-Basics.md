# ⚡ C++ Syntax for DSA — Your Cheat Sheet

> Keep this open while solving. Includes the tricky bits — `sort(rbegin, rend)`, maps, sets, priority queues — all explained with the "why."

---

## 1. The Boilerplate (every program starts here)

```cpp
#include <bits/stdc++.h>     // includes EVERYTHING (vector, map, set, algorithm...)
using namespace std;         // so you write cout, not std::cout

int main() {
    // optimize cin/cout speed for fast input/output (Crucial for online judges)
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    // your code here
    cout << "Hello" << "\n";
    return 0;
}
```

> 🔑 `#include <bits/stdc++.h>` is a shortcut header that pulls in all standard libraries at once.

---

## 2. Variables & Types

```cpp
int a = 5;                   // 32-bit whole number
long long big = 1000000000;  // 64-bit huge whole number (use for big sums!)
double d = 3.14;             // 64-bit decimal
char c = 'a';                // single character (single quotes)
bool flag = true;            // true / false
string s = "hello";          // text (double quotes)
```

> ⚠️ Use `long long` when sums might exceed $\approx 2 \times 10^9$, otherwise, your calculations will overflow.

---

## 3. Printing & Reading

```cpp
cout << x << endl;           // print with new line (and flushes buffer, slower)
cout << a << " " << b << "\n";  // faster, preferred for coding platforms

cin >> x;                    // read one value
cin >> a >> b;               // read multiple values
```

---

## 4. Vectors (dynamic arrays)

```cpp
vector<int> v;               // empty
vector<int> v(5);            // 5 zeros
vector<int> v(5, 1);         // five 1's
vector<int> v = {10, 20, 30};// direct values

v.push_back(99);             // append to end
v.size();                    // number of elements
v[0] = 5;                    // set
int x = v[2];                // read
v.pop_back();                // remove last from end

// loops
for (int i = 0; i < v.size(); i++) { ... }
for (int x : v) { ... }      // read-only walk
for (int& x : v) { ... }     // write-enabled walk (by reference)

// 2D vector (grid)
vector<vector<int>> grid(3, vector<int>(4, 0));  // 3x4 of zeros
grid[1][2] = 7;
```

---

## 5. Strings

```cpp
string s = "hello";
s.size();  // or s.length()   — both work
s[2];                        // character at index 2 (like an array)
s.substr(1, 3);              // 3 chars starting at index 1: "ell"
s + "world";                 // concatenation (creates new copy)
s.push_back('!');            // append char to end
reverse(s.begin(), s.end()); // reverse in-place
sort(s.begin(), s.end());    // sort characters ascending
s == "hello";                // compares contents directly (returns bool)
```

> ✅ Unlike Java, in C++ you **can** use `==` to compare string contents directly.

---

## 6. The `char` ↔ `int` Trick (crucial for strings)

```cpp
int pos = c - 'a';                   // 'a'→0, 'b'→1, ... 'z'→25
char back = 'a' + pos;               // 0→'a', 1→'b', ...
int digit = c - '0';                 // '7' → 7

// 26-size frequency array
vector<int> freq(26, 0);
freq[c - 'a']++;                     // count this letter
```

---

## 7. unordered_map (key → value) & unordered_set

```cpp
// 1. unordered_map: O(1) average lookup
unordered_map<char, int> mp;
mp['a'] = 1;                         // add / overwrite
mp['a'];                             // read (auto-creates with value 0 if missing!)
mp.count('a');                       // returns 1 if key exists, else 0
mp.erase('a');                       // delete key

// counting loop
mp[c]++;                             // if c is new, it auto-initializes to 0, then increments to 1

// 2. unordered_set: O(1) uniqueness check
unordered_set<int> st;
st.insert(5);                        // add
st.count(5);                         // returns 1 if present, else 0
st.erase(5);
```

---

## 8. Advanced STL Collections for DSA

### A. deque (Double-Ended Queue)
Supports $O(1)$ insertions and deletions at both the front and the back.
```cpp
deque<int> dq;
dq.push_back(10);
dq.push_front(20);
dq.pop_back();
dq.pop_front();
dq.front();                          // peek front
dq.back();                           // peek back
```

### B. priority_queue (Heap)
```cpp
// 1. Max Heap (Default): largest element on top
priority_queue<int> maxHeap;
maxHeap.push(10);
maxHeap.push(5);
maxHeap.top();                       // Returns 10 (largest)
maxHeap.pop();                       // Removes 10

// 2. Min Heap: smallest element on top
priority_queue<int, vector<int>, greater<int>> minHeap;
minHeap.push(10);
minHeap.push(5);
minHeap.top();                       // Returns 5 (smallest)
```

### C. set & map (Sorted Set / Map)
Implemented using red-black trees under the hood. Keeps elements sorted at all times.
```cpp
// 1. set
set<int> s;
s.insert(10);
s.insert(5);
*s.begin();                          // Returns 5 (smallest)
*s.rbegin();                         // Returns 10 (largest)

// 2. map
map<int, string> m;
m[10] = "apple";
m[5] = "banana";
m.begin()->first;                    // Returns 5 (smallest key)
```

---

## 9. Time Complexities STL Cheat-Sheet

| Structure | Method | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **vector** | `operator[]` | Index access | $O(1)$ |
| | `push_back` / `pop_back` | End insert/delete | $O(1)$ amortized |
| | `insert` / `erase` | Middle insert/delete | $O(n)$ |
| **unordered_map/set**| `count` / `insert` / `erase` | HashTable access | $O(1)$ average, $O(n)$ worst |
| **map / set** | `find` / `insert` / `erase` | Balanced Tree access | $O(\log n)$ |
| **priority_queue** | `push` / `pop` | Heap update | $O(\log n)$ |
| | `top` | Heap peek | $O(1)$ |

---

## 10. Sorting Algorithms & Comparators

```cpp
sort(v.begin(), v.end());            // ascending (smallest first)
sort(v.rbegin(), v.rend());          // DESCENDING (largest first)
```

### Visualizing Iterators
- `v.begin()` points to the first element; `v.end()` points to one slot past the last element.
- `v.rbegin()` and `v.rend()` are reverse iterators. Sorting backwards yields descending order.

### Custom Comparators (Lambda syntax)
```cpp
// Sort a vector of intervals (pairs) in ascending order of their start values
vector<pair<int, int>> intervals = {{1,3}, {2,6}, {8,10}};
sort(intervals.begin(), intervals.end(), [](const pair<int,int>& a, const pair<int,int>& b) {
    return a.first < b.first; // Return true if 'a' should come BEFORE 'b'
});
```

---

## 11. Handy Utilities

```cpp
max(a, b);   min(a, b);   abs(x);
swap(a, b);                          // swap two variables
INT_MAX;   INT_MIN;                  // for min/max tracking (need <climits>)
to_string(123);                      // number → string
stoi("123");                         // string → number
*max_element(v.begin(), v.end());    // biggest in a vector
*min_element(v.begin(), v.end());    // smallest in a vector
accumulate(v.begin(), v.end(), 0);   // sum of a vector (need <numeric>)
```

---

## 12. Pairs (combining two values)

```cpp
pair<int, char> p = {5, 'a'};
p.first;    // 5
p.second;   // 'a'

vector<pair<int,int>> vp;
vp.push_back({3, 4});
```
