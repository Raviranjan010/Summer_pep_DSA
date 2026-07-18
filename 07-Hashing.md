# 🗃️ 07 — Hashing (HashSet & HashMap)

> **Explain Like I'm 5:** Imagine a coat-check counter. You hand over your coat, get a ticket, and later that ticket instantly locates your exact coat — without the clerk scanning through every single coat in the building. 
> A **hash** is that instant-lookup ticket. Instead of scanning a whole array ($O(n)$ time), you ask "is it there?" and get an answer *instantly* ($O(1)$ time).

---

## ⚙️ Under the Hood: How Hashing Works

A HashTable (the engine behind HashMap and HashSet) maps keys to values using a combination of a **Hash Function** and a **Bucket Array**.

### 1. The Hash Function
A mathematical function that converts any key (like a string `"apple"` or integer `482`) into an integer index within the range of our bucket array:

$$\text{Bucket Index} = \text{Hash}(Key) \pmod{\text{Bucket Size}}$$

A good hash function distributes keys uniformly across the array to avoid clusters.

### 2. Collisions & Collision Resolution
Since there are infinite possible keys but a finite number of buckets, two different keys will eventually hash to the exact same index. This is a **Collision**.
There are two primary ways to resolve collisions:

#### Method A: Separate Chaining (Used by Java's HashMap)
Each slot in the bucket array points to a linked list (or a balanced tree) of elements that hashed to that same index.

```
Bucket Array:
┌───┐
│ 0 │ ──► [ Key: "apple", Val: 10 ] ──► NULL
├───┤
│ 1 │ ──► NULL
├───┤
│ 2 │ ──► [ Key: "banana", Val: 3 ] ──► [ Key: "cherry", Val: 7 ] ──► NULL
├───┤
│ 3 │ ──► NULL
└───┘
```

#### Method B: Open Addressing (Linear Probing)
If a slot is full, the computer searches sequentially for the next available empty slot in the array.

### 3. Load Factor & Rehashing
The **Load Factor ($\alpha$)** measures how full the HashTable is:

$$\alpha = \frac{\text{Number of Stored Keys}}{\text{Total Bucket Size}}$$

- When $\alpha$ exceeds a threshold (typically **`0.75`**), lookup performance begins to degrade because collision chains grow longer.
- The table triggers **Rehashing**: it doubles the bucket array size, creates a new hash function modulo, and moves all elements to their new locations. This is an $O(n)$ operation but happens infrequently, maintaining an **amortized $O(1)$** insertion time.

---

## 📊 HashSet vs. HashMap

| Structure | Purpose | Internal Mechanism | Typical Question |
| :--- | :--- | :--- | :--- |
| **HashSet** | Stores unique keys. | A HashMap under the hood with a dummy value. | *"Have I seen this element before?"* |
| **HashMap** | Maps unique keys to values. | A table of Key-Value pairs. | *"What information is associated with this key?"* |

---

## 🎯 Practice Problems & Worked Solutions

Work through these problems in order to see how Hashing transforms nested loops into linear lookups.

| # | Problem | Difficulty | Link | Key Idea |
|---|---|---|---|---|
| 1 | Contains Duplicate | Easy | [LeetCode](https://leetcode.com/problems/contains-duplicate/) | HashSet uniqueness check. |
| 2 | Two Sum | Easy | [LeetCode](https://leetcode.com/problems/two-sum/) | Map target complements to indices. |
| 3 | Isomorphic Strings | Easy | [LeetCode](https://leetcode.com/problems/isomorphic-strings/) | Two-way mapping check. |
| 4 | Find Common Characters | Easy | [LeetCode](https://leetcode.com/problems/find-common-characters/) | Intersecting frequency counts. |
| 5 | Sort Characters By Frequency | Medium | [LeetCode](https://leetcode.com/problems/sort-characters-by-frequency/) | Count frequencies, sort keys by value. |
| 6 | Find The Difference | Easy | [LeetCode](https://leetcode.com/problems/find-the-difference/) | Balance counts or cancel out using XOR. |

---

### Solution 1: Contains Duplicate

**Intuition:** 
As we traverse the array, check if the current element is already in our `seen` set.
- If it is, we found a duplicate -> return `true`.
- Otherwise, add the element to the set and continue.

**Complexity:**
- **Time:** $O(n)$ — Single pass traversal.
- **Space:** $O(n)$ — In the worst case, we store all $n$ unique elements in the set.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean containsDuplicate(int[] nums) {
    HashSet<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (seen.contains(num)) {
            return true; // Found duplicate
        }
        seen.add(num);
    }
    return false;
}
```

#### Python
```python
def contains_duplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return True # Found duplicate
        seen.add(num)
    return False
```

#### C++
```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> seen;
    for (int num : nums) {
        if (seen.count(num)) {
            return true; // Found duplicate
        }
        seen.insert(num);
    }
    return false;
}
```
</details>

---

### Solution 2: Two Sum

**Intuition:**
For each number `nums[i]`, we need to find if its complement `target - nums[i]` exists in the array.
Instead of checking every pair ($O(n^2)$), we store each visited number and its index in a HashMap. At each step, we look up if `target - nums[i]` is in the map. If it is, we return their indices.

**Complexity:**
- **Time:** $O(n)$ — One-pass traversal.
- **Space:** $O(n)$ — Store elements in the HashMap.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<>(); // Value -> Index
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{};
}
```

#### Python
```python
def two_sum(nums, target):
    mp = {} # Value -> Index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in mp:
            return [mp[complement], i]
        mp[num] = i
    return []
```

#### C++
```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> mp; // Value -> Index
    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if (mp.count(complement)) {
            return {mp[complement], i};
        }
        mp[nums[i]] = i;
    }
    return {};
}
```
</details>

---

### Solution 3: Isomorphic Strings

**Intuition:**
A mapping must exist both ways. For example, if `s = "egg"` and `t = "add"`, then `'e'` maps to `'a'` and `'g'` maps to `'d'`. Additionally, `'a'` must map back to `'e'` and `'d'` back to `'g'`. We use two HashMaps to track these mappings.

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$ (The character set is bounded by ASCII size, at most 256 keys).

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean isIsomorphic(String s, String t) {
    HashMap<Character, Character> sToT = new HashMap<>();
    HashMap<Character, Character> tToS = new HashMap<>();
    
    for (int i = 0; i < s.length(); i++) {
        char a = s.charAt(i);
        char b = t.charAt(i);
        
        if (sToT.containsKey(a) && sToT.get(a) != b) return false;
        if (tToS.containsKey(b) && tToS.get(b) != a) return false;
        
        sToT.put(a, b);
        tToS.put(b, a);
    }
    return true;
}
```

#### Python
```python
def is_isomorphic(s, t):
    s_to_t, t_to_s = {}, {}
    for a, b in zip(s, t):
        if a in s_to_t and s_to_t[a] != b:
            return False
        if b in t_to_s and t_to_s[b] != a:
            return False
        s_to_t[a] = b
        t_to_s[b] = a
    return True
```

#### C++
```cpp
bool isIsomorphic(string s, string t) {
    unordered_map<char, char> sToT, tToS;
    for (int i = 0; i < s.size(); i++) {
        char a = s[i], b = t[i];
        if (sToT.count(a) && sToT[a] != b) return false;
        if (tToS.count(b) && tToS[b] != a) return false;
        sToT[a] = b;
        tToS[b] = a;
    }
    return true;
}
```
</details>

---

### Solution 4: Find Common Characters

**Intuition:**
For each string, calculate the frequency count of each character. Keep a global minimum frequency list `minFreq` of size 26. For each letter, update `minFreq[c] = min(minFreq[c], currentWordFreq[c])`.

**Complexity:**
- **Time:** $O(n \times L)$ where $n$ is the number of words, and $L$ is the average word length.
- **Space:** $O(1)$ auxiliary space (frequency arrays are fixed at size 26).

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public List<String> commonChars(String[] words) {
    int[] minFreq = new int[26];
    Arrays.fill(minFreq, Integer.MAX_VALUE);
    
    for (String w : words) {
        int[] freq = new int[26];
        for (char c : w.toCharArray()) {
            freq[c - 'a']++;
        }
        for (int i = 0; i < 26; i++) {
            minFreq[i] = Math.min(minFreq[i], freq[i]);
        }
    }
    
    List<String> result = new ArrayList<>();
    for (int i = 0; i < 26; i++) {
        while (minFreq[i] > 0) {
            result.add(String.valueOf((char) ('a' + i)));
            minFreq[i]--;
        }
    }
    return result;
}
```

#### Python
```python
from collections import Counter

def common_chars(words):
    # Intersect frequencies using Counter & intersection
    common = Counter(words[0])
    for w in words[1:]:
        common &= Counter(w)
    return list(common.elements())
```

#### C++
```cpp
vector<string> commonChars(vector<string>& words) {
    vector<int> minFreq(26, INT_MAX);
    for (string& w : words) {
        vector<int> freq(26, 0);
        for (char c : w) {
            freq[c - 'a']++;
        }
        for (int i = 0; i < 26; i++) {
            minFreq[i] = min(minFreq[i], freq[i]);
        }
    }
    vector<string> result;
    for (int i = 0; i < 26; i++) {
        while (minFreq[i] > 0) {
            result.push_back(string(1, 'a' + i));
            minFreq[i]--;
        }
    }
    return result;
}
```
</details>

---

### Solution 5: Sort Characters By Frequency

**Intuition:**
Use a HashMap to build character frequency counts. Then, sort the character keys based on their values (frequencies) in descending order. Construct the result string by appending each character repeated by its count.

**Complexity:**
- **Time:** $O(n + k \log k)$ where $n$ is string length, and $k$ is unique characters count ($k \le 256$, so essentially $O(n)$).
- **Space:** $O(n)$ (for storing frequencies and building the string).

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public String frequencySort(String s) {
    HashMap<Character, Integer> counts = new HashMap<>();
    for (char c : s.toCharArray()) {
        counts.put(c, counts.getOrDefault(c, 0) + 1);
    }
    
    // Sort keys based on count value
    List<Character> chars = new ArrayList<>(counts.keySet());
    chars.sort((a, b) -> counts.get(b) - counts.get(a));
    
    StringBuilder sb = new StringBuilder();
    for (char c : chars) {
        int count = counts.get(c);
        for (int i = 0; i < count; i++) {
            sb.append(c);
        }
    }
    return sb.toString();
}
```

#### Python
```python
from collections import Counter

def frequency_sort(s):
    freq = Counter(s)
    # most_common() returns list of (element, count) sorted by count descending
    return "".join(c * count for c, count in freq.most_common())
```

#### C++
```cpp
string frequencySort(string s) {
    unordered_map<char, int> freq;
    for (char c : s) freq[c]++;
    
    vector<pair<int, char>> arr;
    for (auto p : freq) {
        arr.push_back({p.second, p.first});
    }
    // Sort in descending order
    sort(arr.rbegin(), arr.rend());
    
    string result = "";
    for (auto p : arr) {
        result += string(p.first, p.second);
    }
    return result;
}
```
</details>

---

### Solution 6: Find The Difference

**Intuition:**
1. **Counting method:** Count frequencies in `t`, subtract using characters in `s`. The character left with count $> 0$ is the extra character.
2. **XOR method:** Since $X \oplus X = 0$, XORing all characters from both strings will cancel out duplicate characters, leaving only the extra one.

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java (XOR Version)
```java
public char findTheDifference(String s, String t) {
    char extra = 0;
    for (int i = 0; i < s.length(); i++) {
        extra ^= s.charAt(i);
    }
    for (int i = 0; i < t.length(); i++) {
        extra ^= t.charAt(i);
    }
    return extra;
}
```

#### Python (XOR Version)
```python
def find_the_difference(s, t):
    extra = 0
    for ch in s:
        extra ^= ord(ch)
    for ch in t:
        extra ^= ord(ch)
    return chr(extra)
```

#### C++ (XOR Version)
```cpp
char findTheDifference(string s, string t) {
    char extra = 0;
    for (char c : s) extra ^= c;
    for (char c : t) extra ^= c;
    return extra;
}
```
</details>

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Worst Case Performance:**
   - In rare situations where your hash function distributes all keys to the same bucket index, a HashMap's lookup degrades from $O(1)$ to $O(n)$. Java 8 resolves this by converting collision linked lists into Red-Black trees when a list's length exceeds 8, keeping worst-case lookup at $O(\log n)$.

2. **Modifying Keys While inside the Map:**
   - Never modify an object after using it as a key in a HashMap. Doing so changes the object's hash code, making it impossible for the HashMap to locate it again, resulting in memory leaks and bugs.

---

> 👉 Next, open `08-Prefix-Sum.md` to learn how precomputations unlock range query optimization! 💪
