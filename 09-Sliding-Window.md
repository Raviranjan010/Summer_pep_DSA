# 🪟 09 — Sliding Window (The Slow & Clear Guide)

> 📣 **If you've been lost for two days — start here and read every line slowly. Do NOT skip the boxes.** By the end you'll wonder why it ever felt hard. We're going to build it one tiny step at a time. No step is skipped. Promise. 💛

---

## 🧠 Part 1 — The ONE Idea (read this 3 times)

Forget code for a minute. Here's the whole topic in a picture.

You have marks: `[1, 2, 3, 4, 5]`. Find the **sum of every group of 3 in a row**.

```
[1, 2, 3] 4  5   →  1+2+3 = 6
 1 [2, 3, 4] 5   →  2+3+4 = 9
 1  2 [3, 4, 5]  →  3+4+5 = 12
```

The **slow way**: add 3 numbers, every single time. That's wasteful.

Now look closely at how the window *moves* from the first group to the second:

```
[1, 2, 3]  →  [2, 3, 4]
```

> ❓ What actually changed?
> - The **1 left** (it slid out the left side) 👋
> - The **4 joined** (it slid in on the right side) 🤝
> - The `2` and `3` **never moved** — they're still in the window!

So instead of re-adding everything, just do:

> ## ⭐ new sum = old sum − outgoing + incoming
> `9 = 6 − 1 + 4`

That's it. **That single line is the entire topic.** A "window" is a group we slide across the array, updating it cheaply instead of recomputing. 🪟

> 🧊 **Frozen-in-your-brain version:**
> **Remove the one leaving. Add the one entering. Keep the rest.**

---

## 🪟 Part 2 — Two Types of Windows (know which one you're in)

There are only **two kinds** of sliding window. Every problem is one of these:

| Type | The window size is… | You're asked… | Example |
|------|--------------------|---------------|---------|
| **Fixed** | given to you (a number `k`) | best/count of size-k groups | "biggest sum of any 3 in a row" |
| **Variable** | *not* given — it grows & shrinks | longest/shortest that follows a rule | "longest run with no repeats" |

> 🎯 **Before writing ANY code, ask: is my window size fixed (given a `k`)? Or does it grow/shrink based on a rule?** Answering this tells you which template to use. That one question removes 90% of the confusion.

---

# 🟦 PART A — FIXED WINDOW

> The size `k` is handed to you. The window is always exactly `k` wide. It just slides right, one step at a time.

## The Recipe (memorize these 3 steps)

> 1. **Build** the first window (add the first `k` elements).
> 2. **Slide**: for each new element → `add the incoming, remove the outgoing`.
> 3. **Update** your answer (max? count?) after each slide.

```
Build first window        Then slide, one step at a time:
┌─────────┐               ┌─────────┐
│ k items │  ...          x │ k items │ ...
└─────────┘               └─────────┘
                          ↑ remove this   ↑ add this
```

---

## 🟢 A1 — Maximum Average Subarray `(LC 643)`

> **Story:** A teacher wants the best group of 4 students in a row (highest total marks). Find it.
> `marks = [1, 12, -5, -6, 50, 3]`, `k = 4`

**Watch the window slide (this dry run is the heart of everything — follow every line):**

```
Build first window: 1 + 12 + (-5) + (-6) = 2     → maxSum = 2

Slide → remove 1, add 50:   2 − 1 + 50 = 51       → maxSum = 51
Slide → remove 12, add 3:  51 − 12 + 3 = 42       → maxSum = 51 (42 is smaller)

Best sum = 51.  Average = 51 / 4 = 12.75  ✅
```

<details>
<summary>☕ Java</summary>

```java
public double findMaxAverage(int[] nums, int k) {
    int sum = 0;
    for (int i = 0; i < k; i++) sum += nums[i];   // STEP 1: build first window
    int maxSum = sum;

    for (int i = k; i < nums.length; i++) {
        sum += nums[i];          // STEP 2a: add the incoming (right)
        sum -= nums[i - k];      // STEP 2b: remove the outgoing (left)
        maxSum = Math.max(maxSum, sum);            // STEP 3: update answer
    }
    return (double) maxSum / k;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def findMaxAverage(self, nums, k):
    window = sum(nums[:k])          # STEP 1: build first window
    ans = window
    for i in range(k, len(nums)):
        window += nums[i]           # STEP 2a: add the incoming
        window -= nums[i - k]       # STEP 2b: remove the outgoing
        ans = max(ans, window)      # STEP 3: update answer
    return ans / k
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
double findMaxAverage(vector<int>& nums, int k) {
    int sum = 0;
    for (int i = 0; i < k; i++) sum += nums[i];    // STEP 1: build first window
    int mx = sum;
    for (int i = k; i < nums.size(); i++) {
        sum += nums[i];         // STEP 2a: add the incoming
        sum -= nums[i - k];     // STEP 2b: remove the outgoing
        mx = max(mx, sum);      // STEP 3: update answer
    }
    return (double) mx / k;
}
```
</details>

> 🔑 **The magic line is `sum += nums[i]; sum -= nums[i - k];`** — `nums[i]` is entering, `nums[i-k]` is the one exactly `k` steps back that's leaving. Stare at `i - k` until it clicks: if the window is size `k`, the element leaving is `k` positions behind the one entering.

> ⏱️ Time O(n) · Space O(1)

---

## 🟢 A2 — Maximum Number of Vowels `(LC 1456)`

> 😲 **Here's the important part:** this is the **EXACT same problem** as A1. The *only* thing that changed is **what we count** — vowels instead of a sum.
> `s = "abciiidef"`, `k = 3`

```
Window "abc" → vowels: a         → count 1, max 1
Slide "bci"  → remove a, add i   → count 1, max 1
Slide "cii"  → remove b, add i   → count 2, max 2
Slide "iii"  → remove c, add i   → count 3, max 3   ✅
```

<details>
<summary>☕ Java</summary>

```java
public int maxVowels(String s, int k) {
    int count = 0;
    for (int i = 0; i < k; i++)                    // STEP 1: build first window
        if (isVowel(s.charAt(i))) count++;
    int max = count;

    for (int i = k; i < s.length(); i++) {
        if (isVowel(s.charAt(i))) count++;         // incoming is a vowel? +1
        if (isVowel(s.charAt(i - k))) count--;     // outgoing was a vowel? −1
        max = Math.max(max, count);                // STEP 3: update answer
    }
    return max;
}
private boolean isVowel(char ch) {
    return ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u';
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def maxVowels(self, s, k):
    vowels = set('aeiou')
    count = sum(1 for i in range(k) if s[i] in vowels)   # build first window
    ans = count
    for i in range(k, len(s)):
        if s[i] in vowels: count += 1          # incoming vowel? +1
        if s[i - k] in vowels: count -= 1      # outgoing vowel? −1
        ans = max(ans, count)
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
bool isVowel(char c) {
    return c=='a'||c=='e'||c=='i'||c=='o'||c=='u';
}
int maxVowels(string s, int k) {
    int count = 0;
    for (int i = 0; i < k; i++)                    // build first window
        if (isVowel(s[i])) count++;
    int mx = count;
    for (int i = k; i < s.size(); i++) {
        if (isVowel(s[i])) count++;         // incoming vowel? +1
        if (isVowel(s[i - k])) count--;     // outgoing vowel? −1
        mx = max(mx, count);
    }
    return mx;
}
```
</details>

> 🧠 **See the pattern?** Same skeleton as A1. We swapped "add the number" for "add 1 if it's a vowel." *That's the whole difference.* If you understood A1, you already understand A2.

> ⏱️ Time O(n) · Space O(1)

---

## 🟢 A3 — Count Valid Subarrays `(LC 1343)`

> **What changed this time? Almost nothing.** Now we *count* how many windows of size `k` have an average `≥ threshold`.
> `arr = [2,2,2,2,5,5,5,8]`, `k = 3`, `threshold = 4`

> 🪄 **The one clever trick:** "average ≥ threshold" is annoying because of division. Multiply both sides:
> `sum / k ≥ threshold` → `sum ≥ k × threshold`. **No division needed** — just compare the window sum to `k × threshold`.

```
target = k × threshold = 3 × 4 = 12

Window 2+2+2 = 6  → 6 ≥ 12? no    count 0
Window 2+2+2 = 6  → no             count 0
Window 2+2+5 = 9  → no             count 0
Window 2+5+5 = 12 → 12 ≥ 12? yes   count 1
Window 5+5+5 = 15 → yes            count 2
Window 5+5+8 = 18 → yes            count 3   ✅
```

<details>
<summary>☕ Java</summary>

```java
public int numOfSubarrays(int[] arr, int k, int threshold) {
    int target = k * threshold;    // the no-division trick
    int sum = 0, count = 0;

    for (int i = 0; i < arr.length; i++) {
        sum += arr[i];             // add incoming
        if (i >= k) sum -= arr[i - k];             // remove outgoing (once window is full)
        if (i >= k - 1 && sum >= target) count++;  // window is size k AND valid → count it
    }
    return count;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def numOfSubarrays(self, arr, k, threshold):
    target = k * threshold
    window_sum = 0
    count = 0
    for i in range(len(arr)):
        window_sum += arr[i]                # add incoming
        if i >= k:
            window_sum -= arr[i - k]        # remove outgoing
        if i >= k - 1 and window_sum >= target:
            count += 1                      # size k AND valid
    return count
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int numOfSubarrays(vector<int>& arr, int k, int threshold) {
    int target = k * threshold;
    int sum = 0, count = 0;
    for (int i = 0; i < arr.size(); i++) {
        sum += arr[i];                      // add incoming
        if (i >= k) sum -= arr[i - k];      // remove outgoing
        if (i >= k - 1 && sum >= target) count++;  // size k AND valid
    }
    return count;
}
```
</details>

> 🔑 The check `i >= k - 1` means "the window has finally reached full size `k`." Before that, we're still filling the first window and shouldn't count yet.

> ⏱️ Time O(n) · Space O(1)

---

## 🟦 Fixed Window — Board Summary

```
LC 643   Window Sum          →  find MAXIMUM
LC 1456  Window Vowel Count   →  find MAXIMUM
LC 1343  Window Sum           →  COUNT valid windows
```

> All three are the *same three steps*: **build → slide (remove left, add right) → update.** Only "what we track" and "what we do with it" changed. 🎯

---

# 🟨 PART B — VARIABLE WINDOW

> 🚨 **This is where most students get stuck. Read extra slowly.**
>
> Now nobody gives us a size `k`. The window **grows** when things are fine, and **shrinks** when a rule breaks. Two pointers, `left` and `right`, mark the window's edges.

## The New Recipe

> 1. `right` moves forward every step → the window **grows** (add `nums[right]`).
> 2. If a **rule breaks**, move `left` forward to **shrink** until the rule is happy again.
> 3. **Update** the answer (longest? shortest? biggest sum?).

```
   left→                    ←right
   ┌──────────────────────────┐
   │  the window can grow/shrink │
   └──────────────────────────┘
   right always moves right.
   left only moves when we must fix a broken rule.
```

> 💡 **The mental model:** `right` is greedy — it always wants more. `left` is the cleanup crew — it only steps in when `right` caused a problem.

---

## 🟠 B1 — Minimum Size Subarray Sum `(LC 209)`

> **Find the SHORTEST subarray whose sum is `≥ target`.** Window grows to reach the target, then shrinks to stay minimal.
> `target = 7`, `nums = [2,3,1,2,4,3]`

```
right adds until sum ≥ 7, then left shrinks while still ≥ 7:

[2]              sum 2
[2,3]            sum 5
[2,3,1]          sum 6
[2,3,1,2]        sum 8 ≥7 → length 4, try shrink → remove 2 → [3,1,2] sum 6 <7 stop
[3,1,2,4]        sum 10 ≥7 → length 4 → shrink [1,2,4] sum 7 ≥7 length 3 → shrink [2,4] sum 6 stop
[2,4,3]          sum 9 ≥7 → shrink [4,3] sum 7 length 2 ✅ → shrink [3] sum 3 stop

shortest = 2   ([4,3])
```

<details>
<summary>☕ Java</summary>

```java
public int minSubArrayLen(int target, int[] nums) {
    int left = 0, sum = 0, ans = Integer.MAX_VALUE;
    for (int right = 0; right < nums.length; right++) {
        sum += nums[right];                    // grow: add right
        while (sum >= target) {                // rule met → try to shrink
            ans = Math.min(ans, right - left + 1);
            sum -= nums[left];                 // shrink: remove left
            left++;
        }
    }
    return ans == Integer.MAX_VALUE ? 0 : ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def minSubArrayLen(self, target, nums):
    left = 0
    total = 0
    ans = float('inf')
    for right in range(len(nums)):
        total += nums[right]                   # grow: add right
        while total >= target:                 # rule met → shrink
            ans = min(ans, right - left + 1)
            total -= nums[left]                # shrink: remove left
            left += 1
    return 0 if ans == float('inf') else ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int minSubArrayLen(int target, vector<int>& nums) {
    int left = 0, sum = 0, ans = INT_MAX;
    for (int right = 0; right < nums.size(); right++) {
        sum += nums[right];                    // grow: add right
        while (sum >= target) {                // rule met → shrink
            ans = min(ans, right - left + 1);
            sum -= nums[left++];               // shrink: remove left
        }
    }
    return ans == INT_MAX ? 0 : ans;
}
```
</details>

> 🔑 **Window length = `right - left + 1`.** Memorize this — it's how you measure a variable window every time.

> ⏱️ Time O(n) · Space O(1)

---

## 🟠 B2 — Max Consecutive Ones III `(LC 1004)`

> **Longest run of 1s if you may flip at most `k` zeros.** Same as: longest window with **at most `k` zeros**. Window grows; when zeros exceed `k`, shrink.
> `nums = [1,1,0,0,1,1,1]`, `k = 1`

<details>
<summary>☕ Java</summary>

```java
public int longestOnes(int[] nums, int k) {
    int left = 0, zeros = 0, ans = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 0) zeros++;         // grow: track zeros
        while (zeros > k) {                    // rule broken (too many zeros)
            if (nums[left] == 0) zeros--;      // shrink from left
            left++;
        }
        ans = Math.max(ans, right - left + 1); // update longest
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def longestOnes(self, nums, k):
    left = 0
    zeros = 0
    ans = 0
    for right in range(len(nums)):
        if nums[right] == 0:
            zeros += 1                         # grow: track zeros
        while zeros > k:                       # too many zeros → shrink
            if nums[left] == 0:
                zeros -= 1
            left += 1
        ans = max(ans, right - left + 1)       # update longest
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int longestOnes(vector<int>& nums, int k) {
    int left = 0, zeros = 0, ans = 0;
    for (int right = 0; right < nums.size(); right++) {
        if (nums[right] == 0) zeros++;         // grow: track zeros
        while (zeros > k) {                    // too many zeros → shrink
            if (nums[left] == 0) zeros--;
            left++;
        }
        ans = max(ans, right - left + 1);      // update longest
    }
    return ans;
}
```
</details>

> 🧠 Notice: B1 shrank to find the **shortest**; B2 shrinks only to *stay legal*, then measures the **longest**. Same machine, different question.

> ⏱️ Time O(n) · Space O(1)

---

## 🟠 B3 — Longest Substring Without Repeating Characters `(LC 3)`

> **Longest window with all-unique characters.** Use a **Set** to know if a character is already inside. If the new char is a repeat, shrink from the left until it's gone.
> `s = "abcabcbb"` → answer `3` ("abc")

<details>
<summary>☕ Java</summary>

```java
public int lengthOfLongestSubstring(String s) {
    Set<Character> set = new HashSet<>();
    int left = 0, ans = 0;
    for (int right = 0; right < s.length(); right++) {
        while (set.contains(s.charAt(right))) {    // repeat found → shrink
            set.remove(s.charAt(left));
            left++;
        }
        set.add(s.charAt(right));                  // grow: add unique char
        ans = Math.max(ans, right - left + 1);
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def lengthOfLongestSubstring(self, s):
    seen = set()
    left = 0
    ans = 0
    for right in range(len(s)):
        while s[right] in seen:                    # repeat found → shrink
            seen.remove(s[left])
            left += 1
        seen.add(s[right])                         # grow: add unique char
        ans = max(ans, right - left + 1)
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int lengthOfLongestSubstring(string s) {
    unordered_set<char> st;
    int left = 0, ans = 0;
    for (int right = 0; right < s.size(); right++) {
        while (st.count(s[right])) {               // repeat found → shrink
            st.erase(s[left]);
            left++;
        }
        st.insert(s[right]);                       // grow: add unique char
        ans = max(ans, right - left + 1);
    }
    return ans;
}
```
</details>

> ⏱️ Time O(n) · Space O(n)

---

## 🟠 B4 — Fruit Into Baskets `(LC 904)`

> **Longest window with at most 2 different numbers.** Use a **Map** to count how many of each fruit are in the window. When more than 2 types appear, shrink.
> `fruits = [1,2,1,2,3]`

<details>
<summary>☕ Java</summary>

```java
public int totalFruit(int[] fruits) {
    Map<Integer, Integer> map = new HashMap<>();
    int left = 0, ans = 0;
    for (int right = 0; right < fruits.length; right++) {
        map.put(fruits[right], map.getOrDefault(fruits[right], 0) + 1);  // grow
        while (map.size() > 2) {                   // more than 2 types → shrink
            map.put(fruits[left], map.get(fruits[left]) - 1);
            if (map.get(fruits[left]) == 0) map.remove(fruits[left]);
            left++;
        }
        ans = Math.max(ans, right - left + 1);
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def totalFruit(self, fruits):
    count = {}
    left = 0
    ans = 0
    for right in range(len(fruits)):
        count[fruits[right]] = count.get(fruits[right], 0) + 1   # grow
        while len(count) > 2:                      # more than 2 types → shrink
            count[fruits[left]] -= 1
            if count[fruits[left]] == 0:
                del count[fruits[left]]
            left += 1
        ans = max(ans, right - left + 1)
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int totalFruit(vector<int>& fruits) {
    unordered_map<int,int> mp;
    int left = 0, ans = 0;
    for (int right = 0; right < fruits.size(); right++) {
        mp[fruits[right]]++;                        // grow
        while (mp.size() > 2) {                     // more than 2 types → shrink
            mp[fruits[left]]--;
            if (mp[fruits[left]] == 0) mp.erase(fruits[left]);
            left++;
        }
        ans = max(ans, right - left + 1);
    }
    return ans;
}
```
</details>

> 🔑 **Set vs Map:** use a **Set** when you only care "is it present?" (B3). Use a **Map** when you need "how many of each?" (B4), so you know when a type fully leaves the window.

> ⏱️ Time O(n) · Space O(n)

---

## 🟠 B5 — Maximum Erasure Value `(LC 1695)`

> **Biggest SUM of a window with all-unique numbers.** Same as B3 (unique window with a Set), but we also track a running `sum`.
> `nums = [4,2,4,5,6]` → answer `17` ([4,5,6] ... wait, [2,4,5,6] = 17)

<details>
<summary>☕ Java</summary>

```java
public int maximumUniqueSubarray(int[] nums) {
    Set<Integer> set = new HashSet<>();
    int left = 0, sum = 0, ans = 0;
    for (int right = 0; right < nums.length; right++) {
        while (set.contains(nums[right])) {        // repeat → shrink
            set.remove(nums[left]);
            sum -= nums[left];                     // remove its value too
            left++;
        }
        set.add(nums[right]);
        sum += nums[right];                        // add its value
        ans = Math.max(ans, sum);
    }
    return ans;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def maximumUniqueSubarray(self, nums):
    seen = set()
    left = 0
    curr_sum = 0
    ans = 0
    for right in range(len(nums)):
        while nums[right] in seen:                 # repeat → shrink
            seen.remove(nums[left])
            curr_sum -= nums[left]                 # remove its value too
            left += 1
        seen.add(nums[right])
        curr_sum += nums[right]                    # add its value
        ans = max(ans, curr_sum)
    return ans
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
int maximumUniqueSubarray(vector<int>& nums) {
    unordered_set<int> st;
    int left = 0, sum = 0, ans = 0;
    for (int right = 0; right < nums.size(); right++) {
        while (st.count(nums[right])) {            // repeat → shrink
            st.erase(nums[left]);
            sum -= nums[left];                     // remove its value too
            left++;
        }
        st.insert(nums[right]);
        sum += nums[right];                        // add its value
        ans = max(ans, sum);
    }
    return ans;
}
```
</details>

> 🧠 B5 = B3 + a running sum. Once you see B3, this is a 2-line upgrade.

> ⏱️ Time O(n) · Space O(n)

---

## 🟪 PART C — The Bridge Problem: Permutation in String `(LC 567)`

> This one blends **fixed window + frequency counting** (your anagram logic from hashing). Check if any rearrangement of `s1` appears inside `s2`.
> **Idea:** `s1` has a fixed length `k`. Slide a size-`k` window across `s2`. If the window's letter-counts ever exactly match `s1`'s letter-counts → found a permutation.

<details>
<summary>☕ Java</summary>

```java
public boolean checkInclusion(String s1, String s2) {
    if (s1.length() > s2.length()) return false;
    int[] need = new int[26], window = new int[26];
    for (char c : s1.toCharArray()) need[c - 'a']++;   // s1's letter counts
    int k = s1.length();

    for (int i = 0; i < s2.length(); i++) {
        window[s2.charAt(i) - 'a']++;                  // add incoming
        if (i >= k) window[s2.charAt(i - k) - 'a']--;  // remove outgoing
        if (Arrays.equals(need, window)) return true;  // counts match → permutation!
    }
    return false;
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
def checkInclusion(self, s1, s2):
    if len(s1) > len(s2): return False
    need = [0] * 26
    window = [0] * 26
    for ch in s1: need[ord(ch) - ord('a')] += 1        # s1's letter counts
    k = len(s1)
    for i in range(len(s2)):
        window[ord(s2[i]) - ord('a')] += 1             # add incoming
        if i >= k:
            window[ord(s2[i - k]) - ord('a')] -= 1     # remove outgoing
        if window == need:                             # counts match!
            return True
    return False
```
</details>

<details>
<summary>⚡ C++</summary>

```cpp
bool checkInclusion(string s1, string s2) {
    if (s1.size() > s2.size()) return false;
    vector<int> need(26, 0), window(26, 0);
    for (char c : s1) need[c - 'a']++;                 // s1's letter counts
    int k = s1.size();
    for (int i = 0; i < s2.size(); i++) {
        window[s2[i] - 'a']++;                         // add incoming
        if (i >= k) window[s2[i - k] - 'a']--;         // remove outgoing
        if (window == need) return true;               // counts match!
    }
    return false;
}
```
</details>

> ⏱️ Time O(n) · Space O(1) (26 counts)

---

## 🗺️ The Whole Topic on One Page

| Pattern | What's new | Problems |
|---------|-----------|----------|
| **Fixed window + sum** | the base recipe | 643, 1456, 1343 |
| **Fixed window + frequency** | count letters, compare | 567 |
| **Variable window + sum** | grow to reach, shrink to minimize | 209 |
| **Variable window + bad-count** | shrink when a "bad" count exceeds k | 1004 |
| **Set + window** | uniqueness check | 3 |
| **Map + window** | count of each type | 904 |
| **Set + sum + window** | uniqueness + running sum | 1695 |

> 🎓 **Each problem adds exactly ONE new idea to the one before it.** That's why we teach them in order. If a problem feels hard, go back one row — you probably skipped a step.

---

## ✅ Before You Say "I Don't Get It" — Checklist

- [ ] Can I say the golden line? *(new = old − outgoing + incoming)*
- [ ] Can I tell if a problem is **fixed** or **variable** window?
- [ ] For fixed: do I **build → slide → update**?
- [ ] For variable: does `right` grow, and `left` only shrink when a rule breaks?
- [ ] Do I know window length is `right - left + 1`?

> If any box is unticked, that's *exactly* the line to ask me about. A precise question ("why does `left` move here?") gets a fast answer. 🙌

---

# 🎯 Practice (in this order — each builds on the last)

| Problem | Difficulty | Link |
|---------|-----------|------|
| Maximum Average Subarray I | Easy | https://leetcode.com/problems/maximum-average-subarray-i/ |
| Maximum Number of Vowels in a Substring | Medium | https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/ |
| Number of Sub-arrays of Size K and Avg ≥ Threshold | Medium | https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/ |
| Permutation in String | Medium | https://leetcode.com/problems/permutation-in-string/ |
| Minimum Size Subarray Sum | Medium | https://leetcode.com/problems/minimum-size-subarray-sum/ |
| Max Consecutive Ones III | Medium | https://leetcode.com/problems/max-consecutive-ones-iii/ |
| Longest Substring Without Repeating Characters | Medium | https://leetcode.com/problems/longest-substring-without-repeating-characters/ |
| Fruit Into Baskets | Medium | https://leetcode.com/problems/fruit-into-baskets/ |
| Maximum Erasure Value | Medium | https://leetcode.com/problems/maximum-erasure-value/ |

> 🌟 **Do them top to bottom.** Solve 643 until it's boring, THEN move down. Each next problem is a small twist, not a new mountain. That's the secret to sliding window — it only looks like 9 problems; it's really *one idea, dressed 9 ways.* I'm right here for any doubt. 💪 — *Ajai Raj (Mentor)*
