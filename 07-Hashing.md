# 🗃️ 07 — Hashing (HashSet & HashMap)

> 🏷️ **Explain Like I'm 5:** Imagine a coat-check counter. You hand over your coat, get a token, and later that token instantly finds your exact coat — no searching through every coat. A **hash** is that instant-lookup magic. Instead of scanning a whole array (slow), you ask "is it there?" and get an answer *immediately*.

> 💛 This is one of the most-loved topics in interviews because it turns slow O(n²) solutions into fast O(n) ones. Go slow, and watch each problem become a small upgrade of the last.

---

## The 2 Tools You'll Use Forever

| Tool | Question it answers | Real use |
|------|---------------------|----------|
| **HashSet** | *"Have I seen this before?"* | Uniqueness — no duplicates allowed |
| **HashMap** | *"What information is attached to this thing?"* | Mapping / counting — key → value |

> 🧠 **The one-line difference to memorize:**
> **HashSet → Uniqueness.** **HashMap → Mapping / Counting.**

Both give you **O(1) average lookup** — that's the superpower.

---

## Quick Syntax Cheat-Sheet

| Action | ☕ Java | 🐍 Python | ⚡ C++ |
|--------|--------|-----------|--------|
| Make a set | `HashSet<Integer> s = new HashSet<>();` | `s = set()` | `unordered_set<int> s;` |
| Seen it? | `s.contains(x)` | `x in s` | `s.count(x)` |
| Add to set | `s.add(x)` | `s.add(x)` | `s.insert(x)` |
| Make a map | `HashMap<Integer,Integer> m = new HashMap<>();` | `m = {}` | `unordered_map<int,int> m;` |
| Has key? | `m.containsKey(k)` | `k in m` | `m.count(k)` |
| Put / read | `m.put(k,v)` / `m.get(k)` | `m[k] = v` / `m[k]` | `m[k] = v` / `m[k]` |

---

## The Learning Ladder (Why This Order)

Each problem is a **direct upgrade** of the one before — not random:

```
HashSet  →  HashMap  →  Two Maps  →  Frequency Map  →  Frequency + Sorting
   1          2            3              4                  5
```

---

## Problem 1 — Contains Duplicate `(HashSet)`

> **Question it teaches:** *"Have I seen this element before?"*
> Walk the array. Before adding each number, check if it's already in the set. If yes → duplicate found.

<details>
<summary>☕ Java</summary>

```java
static boolean containsDuplicate(int[] nums) {
    HashSet<Integer> set = new HashSet<>();
    for (int num : nums) {
        if (set.contains(num)) return true;   // seen before → duplicate!
        set.add(num);                          // otherwise remember it
    }
    return false;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def containsDuplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return True                        # seen before → duplicate!
        seen.add(num)                          # otherwise remember it
    return False
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> st;
    for (int num : nums) {
        if (st.count(num)) return true;        // seen before → duplicate!
        st.insert(num);                        // otherwise remember it
    }
    return false;
}
```
</details>

> ⏱️ **Time:** O(n) · **Space:** O(n). Compare to checking every pair — that's O(n²). The set is the upgrade.

---

## Problem 2 — Two Sum `(HashMap)` ⭐

> **This is THE HashMap problem.** Ask yourself first: *why* do we need a map, not just a set?
> Because we don't just want "have I seen it" — we want *"where did I see it"* (its index). That extra info = the **value** in the map.

> **Trick:** For each number, the partner we need is `target - num`. If that partner is already in the map, we're done.

```
nums = [2, 7, 11, 15], target = 9
At 2 → need 7 (not seen yet) → store 2
At 7 → need 2 (seen at index 0!) → answer [0, 1]
```

<details>
<summary>☕ Java</summary>

```java
static int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int need = target - nums[i];           // the partner we're looking for
        if (map.containsKey(need))
            return new int[]{map.get(need), i};
        map.put(nums[i], i);                   // store value → its index
    }
    return new int[]{};
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def twoSum(nums, target):
    mp = {}
    for i, num in enumerate(nums):
        need = target - num                    # the partner we're looking for
        if need in mp:
            return [mp[need], i]
        mp[num] = i                            # store value → its index
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> mp;
    for (int i = 0; i < nums.size(); i++) {
        int need = target - nums[i];           // the partner we're looking for
        if (mp.count(need))
            return {mp[need], i};
        mp[nums[i]] = i;                       // store value → its index
    }
    return {};
}
```
</details>

> ⏱️ **Time:** O(n) · **Space:** O(n)

---

## Problem 3 — Isomorphic Strings `(Two HashMaps)`

> **The upgrade:** one map is *not enough*. We must check the mapping **both ways**.
> `s → t` AND `t → s`. If either direction breaks its earlier promise, it's not isomorphic.

```
egg → add
e→a, g→d  ✅  (and a→e, d→g holds too)
```

<details>
<summary>☕ Java</summary>

```java
public boolean isIsomorphic(String s, String t) {
    HashMap<Character, Character> st = new HashMap<>();
    HashMap<Character, Character> ts = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        char a = s.charAt(i), b = t.charAt(i);
        if (st.containsKey(a) && st.get(a) != b) return false;  // s→t broken
        if (ts.containsKey(b) && ts.get(b) != a) return false;  // t→s broken
        st.put(a, b);
        ts.put(b, a);
    }
    return true;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def isIsomorphic(s, t):
    st, ts = {}, {}
    for a, b in zip(s, t):
        if a in st and st[a] != b:
            return False                       # s→t broken
        if b in ts and ts[b] != a:
            return False                       # t→s broken
        st[a] = b
        ts[b] = a
    return True
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
bool isIsomorphic(string s, string t) {
    unordered_map<char,char> st, ts;
    for (int i = 0; i < s.size(); i++) {
        char a = s[i], b = t[i];
        if (st.count(a) && st[a] != b) return false;  // s→t broken
        if (ts.count(b) && ts[b] != a) return false;  // t→s broken
        st[a] = b;
        ts[b] = a;
    }
    return true;
}
```
</details>

> ⏱️ **Time:** O(n) · **Space:** O(1) (at most 256 characters tracked)

---

## Problem 4 — Find Common Characters `(Frequency Counting)`

> **The upgrade:** now we *count* letters in each word, and keep the **minimum** count across all words. A letter that appears in every word — at least this many times — is common to all.

```
bella → l:2, e:1, ...
label → l:2, e:1, ...
roller → l:2, e:1, ...
common: e (min 1), l (min 2)  →  ["e", "l", "l"]
```

<details>
<summary>☕ Java</summary>

```java
public List<String> commonChars(String[] words) {
    int[] minFreq = new int[26];
    Arrays.fill(minFreq, Integer.MAX_VALUE);
    for (String word : words) {
        int[] freq = new int[26];
        for (char c : word.toCharArray()) freq[c - 'a']++;
        for (int i = 0; i < 26; i++)
            minFreq[i] = Math.min(minFreq[i], freq[i]);   // keep the minimum
    }
    List<String> result = new ArrayList<>();
    for (int i = 0; i < 26; i++)
        while (minFreq[i]-- > 0)
            result.add(String.valueOf((char) ('a' + i)));
    return result;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import Counter

def commonChars(words):
    common = Counter(words[0])
    for word in words[1:]:
        common &= Counter(word)                # & keeps the minimum of each
    return list(common.elements())
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
vector<string> commonChars(vector<string>& words) {
    vector<int> mn(26, INT_MAX);
    for (string& w : words) {
        vector<int> cnt(26, 0);
        for (char c : w) cnt[c - 'a']++;
        for (int i = 0; i < 26; i++)
            mn[i] = min(mn[i], cnt[i]);         // keep the minimum
    }
    vector<string> ans;
    for (int i = 0; i < 26; i++)
        while (mn[i]-- > 0)
            ans.push_back(string(1, 'a' + i));
    return ans;
}
```
</details>

> ⏱️ **Time:** O(n · L) (words × word length) · **Space:** O(1) (26 boxes)

---

## Problem 5 — Sort Characters By Frequency `(HashMap + Sorting)`

> **The final upgrade:** count with a map, *then* **sort** by that count. Highest frequency letters come first, repeated their count many times.

```
"tree" → e:2, r:1, t:1  →  "eert" (or "eetr")
```

<details>
<summary>☕ Java</summary>

```java
public String frequencySort(String s) {
    Map<Character, Integer> counts = new HashMap<>();
    for (char ch : s.toCharArray())
        counts.put(ch, counts.getOrDefault(ch, 0) + 1);
    List<Character> chars = new ArrayList<>(counts.keySet());
    chars.sort((a, b) -> counts.get(b) - counts.get(a));   // high → low
    StringBuilder sb = new StringBuilder();
    for (char c : chars) {
        int n = counts.get(c);
        while (n-- > 0) sb.append(c);
    }
    return sb.toString();
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from collections import Counter

def frequencySort(s):
    freq = Counter(s)
    return ''.join(c * n for c, n in freq.most_common())   # most_common sorts for us
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
string frequencySort(string s) {
    unordered_map<char,int> freq;
    for (char c : s) freq[c]++;
    vector<pair<int,char>> arr;
    for (auto p : freq) arr.push_back({p.second, p.first});
    sort(arr.rbegin(), arr.rend());            // sort by count, high → low
    string ans;
    for (auto p : arr) ans += string(p.first, p.second);
    return ans;
}
```
</details>

> ⏱️ **Time:** O(n + k log k) · **Space:** O(n)

---

## 🧠 5-Minute Revision (The Board Summary)

```
HASHSET  →  "Have I seen this before?"  →  Uniqueness
   └─ Contains Duplicate

HASHMAP  →  "What info is attached to this?"  →  Mapping / Counting
   ├─ Two Sum          (value → index)
   ├─ Isomorphic       (char → char, both ways)
   ├─ Common Chars     (char → frequency)
   └─ Frequency Sort   (char → frequency, then sort)
```

> If you can explain this box out loud, you understand hashing. That's the whole topic in one picture. 🎯

---

# 🎯 Practice / Homework

| # | Problem | Difficulty | Practice Link |
|---|---------|-----------|---------------|
| 1 | Find The Difference | Easy | https://leetcode.com/problems/find-the-difference/ |
| 2 | Valid Anagram *(revision)* | Easy | https://leetcode.com/problems/valid-anagram/ |
| 3 | First Unique Character *(revision)* | Easy | https://leetcode.com/problems/first-unique-character-in-a-string/ |
| 4 | Isomorphic Strings *(re-solve)* | Medium | https://leetcode.com/problems/isomorphic-strings/ |
| 5 | Find Common Characters *(re-solve)* | Medium | https://leetcode.com/problems/find-common-characters/ |
| 6 | Sort Characters By Frequency *(re-solve)* | Medium | https://leetcode.com/problems/sort-characters-by-frequency/ |

> 💡 Problems 2 & 3 also live in your **Strings** notes (`05-Strings.md`) — notice they're the *same* frequency-array idea. That repetition is exactly how it locks in. 🙌

---

## ✅ Worked Solution — Find The Difference

> **Problem:** String `t` is `s` shuffled, plus **one extra letter**. Find that letter.
> **Weapon used:** frequency counting — count up for `t`, count down for `s`, the leftover letter is the answer. (A neat XOR trick also exists — shown after.)

<details>
<summary>☕ Java</summary>

```java
char findTheDifference(String s, String t) {
    int[] count = new int[26];
    for (char c : t.toCharArray()) count[c - 'a']++;   // add all of t
    for (char c : s.toCharArray()) count[c - 'a']--;   // remove all of s
    for (int i = 0; i < 26; i++)
        if (count[i] > 0) return (char) ('a' + i);     // the leftover
    return ' ';
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def findTheDifference(s, t):
    count = [0] * 26
    for c in t:
        count[ord(c) - ord('a')] += 1          # add all of t
    for c in s:
        count[ord(c) - ord('a')] -= 1          # remove all of s
    for i in range(26):
        if count[i] > 0:
            return chr(ord('a') + i)           # the leftover
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
char findTheDifference(string s, string t) {
    int count[26] = {0};
    for (char c : t) count[c - 'a']++;         // add all of t
    for (char c : s) count[c - 'a']--;         // remove all of s
    for (int i = 0; i < 26; i++)
        if (count[i] > 0) return 'a' + i;      // the leftover
    return ' ';
}
```
</details>

> ⏱️ **Time:** O(n) · **Space:** O(1)

> 🪄 **Bonus (XOR trick):** XOR every character of both strings together. Identical letters cancel out, leaving only the extra one. `char ^= c` over all chars of `s` and `t`. Clever, but the counting version above is easier to explain in an interview.

> Problems 2 (Valid Anagram) and 3 (First Unique Character) are already fully solved in your **Strings** notes — flip back to `05-Strings.md` for those. 🌟

---

> 🎉 **You've unlocked hashing** — the tool that quietly powers half of all interview solutions. Every time you catch yourself writing a nested loop, pause and ask: *"could a HashSet or HashMap make this one pass?"* That instinct is what separates a beginner from someone interview-ready. I'm right here for any doubt. 💪 — *Ajai Raj (Mentor)*
