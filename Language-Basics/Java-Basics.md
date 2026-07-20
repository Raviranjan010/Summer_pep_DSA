# ☕ Java Syntax for DSA — Your Cheat Sheet

> Keep this open while solving. Every syntax you need for arrays, strings, maps, and sets — with the "why" so it sticks. No need to memorize; just look it up until it's muscle memory.

---

## 1. The Boilerplate (every program starts here)

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        // your code here
        System.out.println("Hello");
    }
}
```

- `import java.util.*;` → brings in ArrayList, HashMap, HashSet, Arrays, Collections. **Always add this for DSA.**
- `System.out.println(x)` → print with a new line. `System.out.print(x)` → no new line.

---

## 2. Variables & Types

```java
int a = 5;               // whole number (32-bit)
long big = 1000000000L;   // huge whole number (64-bit, note the L)
double d = 3.14;          // decimal (64-bit float)
char c = 'a';             // single character (single quotes!)
boolean flag = true;      // true / false
String s = "hello";       // text (double quotes!)
```

> ⚠️ `'a'` (char, single quotes) vs `"a"` (String, double quotes) — a classic beginner mix-up.

---

## 3. Arrays

```java
int[] arr = new int[5];              // 5 zeros: [0,0,0,0,0]
int[] arr2 = {10, 20, 30};           // direct values
int n = arr.length;                  // size (NO brackets — it's a field, not length())
arr[0] = 99;                         // set
int x = arr[2];                      // read

Arrays.fill(arr, 1);                 // fill all with 1
Arrays.sort(arr);                    // sort ascending
int[] copy = arr.clone();            // copy an array

// 2D array
int[][] grid = new int[3][4];        // 3 rows, 4 columns
grid[1][2] = 7;
```

> 🔑 Array size is `arr.length` (no parentheses). String length is `s.length()` (with parentheses).

---

## 4. Loops

```java
// standard for
for (int i = 0; i < n; i++) { ... }

// for-each (read-only walk)
for (int x : arr) { ... }

// while
while (condition) { ... }

// loop backwards
for (int i = n - 1; i >= 0; i--) { ... }
```

---

## 5. Strings

```java
String s = "hello";
int len = s.length();                // length WITH ()
char c = s.charAt(2);                // character at index 2
String sub = s.substring(1, 4);      // chars from index 1 to 3 (4 excluded)
boolean eq = s.equals("hello");      // compare content — NEVER use == for strings!
char[] arr = s.toCharArray();        // string → char array
String back = new String(arr);       // char array → string
String up = s.toUpperCase();
boolean has = s.contains("ell");
```

> ⚠️ **Compare strings with `.equals()`, not `==`.** `==` checks if they point to the exact same object in memory, not if they contain the same text.

### Building strings in a loop → use StringBuilder
```java
StringBuilder sb = new StringBuilder();
sb.append("a");
sb.append(c);
String result = sb.toString();       // convert back when done
```

> 🔑 **Why?** `String` is immutable. Doing `s = s + c` in a loop takes $O(n^2)$ time because of copying. StringBuilder does it in $O(n)$ time.

---

## 6. The `char` ↔ `int` Trick (crucial for strings)

```java
int pos = c - 'a';                   // 'a'→0, 'b'→1, ... 'z'→25
char back = (char) ('a' + pos);      // 0→'a', 1→'b', ...
int digit = c - '0';                 // '7' → 7  (char digit to real number)
```

Used for the 26-size frequency array:
```java
int[] freq = new int[26];
freq[c - 'a']++;                     // count this letter
```

---

## 7. HashMap (key → value) — counting & mapping

```java
HashMap<Character, Integer> map = new HashMap<>();

map.put('a', 1);                     // add / overwrite
map.get('a');                        // read (returns null if missing!)
map.containsKey('a');                // check existence
map.remove('a');
map.size();

// ⭐ getOrDefault — THE counting shortcut
map.put(c, map.getOrDefault(c, 0) + 1);

// loop over a map
for (Map.Entry<Character, Integer> e : map.entrySet()) {
    char key = e.getKey();
    int val = e.getValue();
}
for (char key : map.keySet()) { ... }    // just keys
```

---

## 8. HashSet (unique items) — "have I seen this?"

```java
HashSet<Integer> set = new HashSet<>();

set.add(5);                          // add (duplicates ignored automatically)
set.contains(5);                     // seen it?
set.remove(5);
set.size();

for (int x : set) { ... }            // loop over it
```

---

## 9. ArrayList (a resizable array / list)

```java
List<Integer> list = new ArrayList<>();

list.add(10);                        // append
list.get(0);                         // read by index
list.set(0, 99);                     // overwrite
list.size();
list.remove(0);                      // remove by index
list.contains(10);
Collections.sort(list);              // sort ascending
Collections.sort(list, (a, b) -> b - a);   // sort descending
```

---

## 10. Advanced Collections for DSA

### A. Deque (ArrayDeque) — Used for Stack & Queue
Do not use the legacy `Stack` class (it has slow synchronization overhead). Use `ArrayDeque`.
```java
// 1. Stack behavior (LIFO)
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.peek();                        // Look at top
stack.pop();                         // Remove from top

// 2. Queue behavior (FIFO)
Deque<Integer> queue = new ArrayDeque<>();
queue.offer(10);                     // Add to back
queue.peek();                        // Look at front
queue.poll();                        // Remove from front
```

### B. PriorityQueue — Min/Max Heap
Used for top-K elements, merging arrays, and greedy algorithms.
```java
// 1. Min Heap (Default): smallest values first
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(10);
minHeap.offer(5);
minHeap.peek();                      // Returns 5 (smallest)
minHeap.poll();                      // Removes 5

// 2. Max Heap: largest values first
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
maxHeap.offer(10);
maxHeap.offer(15);
maxHeap.peek();                      // Returns 15 (largest)
```

### C. TreeSet & TreeMap (Sorted Set/Map)
Keeps elements sorted at all times. Search, insert, and delete take $O(\log n)$ time.
```java
// 1. TreeSet
TreeSet<Integer> ts = new TreeSet<>();
ts.add(10);
ts.add(5);
ts.first();                          // Returns 5 (smallest)
ts.last();                           // Returns 10 (largest)
ts.higher(5);                        // Returns 10 (smallest element strictly greater than 5)

// 2. TreeMap
TreeMap<Integer, String> tm = new TreeMap<>();
tm.put(10, "apple");
tm.put(5, "banana");
tm.firstKey();                       // Returns 5 (smallest key)
tm.lastKey();                        // Returns 10 (largest key)
```

---

## 11. Time Complexities Cheat-Sheet

| Data Structure | Operation | Method | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Array** | Access index | `arr[i]` | $O(1)$ |
| | Search element | (Loop) | $O(n)$ |
| **ArrayList** | Access index / Append | `list.get(i)` / `list.add(x)` | $O(1)$ |
| | Insert / Delete (middle)| `list.add(idx, x)` / `list.remove(idx)` | $O(n)$ |
| **HashMap / Set** | Insert / Lookup / Delete | `put` / `get` / `contains` | $O(1)$ average |
| **PriorityQueue** | Insert / Delete Min-Max | `offer` / `poll` | $O(\log n)$ |
| | Peek Min-Max | `peek` | $O(1)$ |
| **TreeSet / Map** | Insert / Lookup / Delete | `add` / `contains` / `put` | $O(\log n)$ |

---

## 12. Sorting with Custom Order (Comparators)

```java
Arrays.sort(arr);                            // ascending (primitives only ascending)
Collections.sort(list);                      // ascending
Collections.sort(list, (a, b) -> a - b);     // ascending
Collections.sort(list, (a, b) -> b - a);     // descending

// Sort 2D arrays (e.g. by start time in interval scheduling)
int[][] intervals = {{1,3}, {2,6}, {8,10}};
Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0])); // Ascending by col 0
```

> 🧠 **Reading a comparator:** `(a, b) -> a - b` means "ascending." Flip to `b - a` for "descending."

---

## 13. Handy Math & Utilities

```java
Math.max(a, b);   Math.min(a, b);   Math.abs(x);
Math.pow(2, 3);                      // returns double 8.0
Integer.MAX_VALUE;  Integer.MIN_VALUE;   // for min/max tracking
String.valueOf(123);                 // number → string
Integer.parseInt("123");             // string → number
```

---

## 14. Reading Input (for online judges / GfG)

```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();                // read one int
String word = sc.next();             // read one word
String line = sc.nextLine();         // read whole line
```

---

## 📋 Step-by-Step Dry Run: Java `HashMap.getOrDefault()`

```java
String str = "aba";
HashMap<Character, Integer> map = new HashMap<>();
for (char c : str.toCharArray()) {
    map.put(c, map.getOrDefault(c, 0) + 1);
}
```

- **Iteration 1 (`c = 'a'`):** `map.getOrDefault('a', 0)` returns `0`. `put('a', 0 + 1 = 1)`. Map state: `{'a': 1}`.
- **Iteration 2 (`c = 'b'`):** `map.getOrDefault('b', 0)` returns `0`. `put('b', 0 + 1 = 1)`. Map state: `{'a': 1, 'b': 1}`.
- **Iteration 3 (`c = 'a'`):** `'a'` exists, returns `1`. `put('a', 1 + 1 = 2)`. Map state: `{'a': 2, 'b': 1}`.

**Final Map:** `{'a': 2, 'b': 1}` ✅

---

## 🎓 Java Viva Questions & Answers

### Q1: What is the difference between JDK, JRE, and JVM?
**Answer:**
- **JDK (Java Development Kit):** Software development kit containing compiler (`javac`), debugger, and tools needed to write and compile Java programs.
- **JRE (Java Runtime Environment):** Implementation of JVM + core class libraries required to run compiled Java bytecode (`.class` files).
- **JVM (Java Virtual Machine):** Abstract computing machine that executes Java bytecode on target operating systems (provides platform independence: "Write Once, Run Anywhere").

### Q2: What is the difference between Primitive types and Reference types in Java?
**Answer:**
- **Primitives (`int`, `long`, `double`, `boolean`, `char`):** Store raw binary values directly on the **Stack**. Non-nullable, fixed memory size.
- **Reference Types (`Objects`, `String`, `Arrays`, `ArrayList`):** Variables on the stack store a memory reference (address) pointing to the actual object stored on the **Heap**. Default value is `null`.

### Q3: Why should `StringBuilder` be used instead of `String` concatenation inside loops?
**Answer:**
Java `String` objects are immutable. Performing `s += c` inside a loop creates a new `String` object and copies characters on every iteration, leading to $O(n^2)$ total time complexity. `StringBuilder` uses a dynamic character buffer that appends characters in amortized $O(1)$ time, taking $O(n)$ overall.

### Q4: How did Java 8 improve `HashMap` worst-case performance?
**Answer:**
Prior to Java 8, hash collisions were handled using single linked lists (worst-case $O(n)$ lookup). In Java 8+, when the number of items in a single hash bucket exceeds a threshold of 8 (and table capacity $\ge 64$), the bucket transitions from a Linked List to a **Red-Black Tree**, improving worst-case search time to $O(\log n)$.

### Q5: What is the difference between `==` and `.equals()` in Java?
**Answer:**
- **`==`:** Reference equality operator. Checks if both variables reference the exact same memory address on the heap.
- **`.equals()`:** Value equality method. Compares the internal logical content of two objects (overridden in `String`, `Integer`, etc.).

