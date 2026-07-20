# 🔤 05 — Strings

> **The primary principle:** A string is simply an array of characters. Almost all array algorithms (Two Pointers, Hashing, Counting) can be applied directly to strings!

---

## ⚔️ Mutability vs. Immutability: Language Design Differences

How strings are handled in memory differs significantly between languages. Knowing these details is critical for passing coding interviews.

### 1. Java & Python (Immutable Strings)
- In Java and Python, string objects are **read-only** (immutable). Once created, their characters cannot be modified in-place.
- **The Loop Concatenation Trap:**
  Doing `s += char` inside a loop of size $n$ takes $O(n^2)$ time!
  ```python
  # TRAP: O(n^2) time complexity
  s = ""
  for char in list_of_chars:
      s += char # Creates a BRAND NEW string copy each time
  ```
  At each step, the computer copies the existing characters to a new memory block. Total operations: $1 + 2 + 3 + \dots + n = \frac{n(n+1)}{2} = O(n^2)$.
- **The Solution:**
  - In **Java**, use **`StringBuilder`**, which behaves like a mutable dynamic array of characters.
  - In **Python**, append characters to a list `[]`, and join them at the end using `''.join(list)`.

### 2. C++ (Mutable Strings)
- In C++, `std::string` is **mutable** (modifiable).
- It is allocated on the heap as a dynamic array of characters, allowing you to modify individual characters in-place (`s[i] = 'x'`) and append elements in amortized $O(1)$ time.

---

## 🧮 The Character Arithmetic Trick: `c - 'a'`

Computers do not store letters; they store numbers (ASCII codes).
- `'a'` is represented as `97`
- `'b'` is represented as `98`
- ...
- `'z'` is represented as `122`

Subtracting the base character `'a'` from any lowercase character maps it directly to a relative index from `0` to `25`:

$$\text{Index} = \text{ASCII}(c) - \text{ASCII}('a')$$

```
c = 'a'  ==>  97 - 97 = 0
c = 'b'  ==>  98 - 97 = 1
c = 'z'  ==>  122 - 97 = 25
```

This maps letters directly to index positions in a fixed-size `26`-element frequency counting array:

```
Index:         0     1     2     3     ...     25
Character:    'a'   'b'   'c'   'd'    ...    'z'
           ┌─────┬─────┬─────┬─────┬─────────┬─────┐
Freq Array:│  2  │  0  │  1  │  0  │   ...   │  1  │
           └─────┴─────┴─────┴─────┴─────────┴─────┘
```

---

## 🎯 Practice Problems & Worked Solutions

Verify your solutions for these 5 core string problems.

| # | Problem | Difficulty | Link | Key Idea |
|---|---|---|---|---|
| 1 | Valid Palindrome | Easy | [LeetCode](https://leetcode.com/problems/valid-palindrome/) | Two pointers skipping non-alphanumeric chars. |
| 2 | Valid Anagram | Easy | [LeetCode](https://leetcode.com/problems/valid-anagram/) | Frequency array subtraction check. |
| 3 | First Unique Character | Easy | [LeetCode](https://leetcode.com/problems/first-unique-character-in-a-string/) | Two-pass frequency check. |
| 4 | Reverse String | Easy | [LeetCode](https://leetcode.com/problems/reverse-string/) | Swap elements in-place. |
| 5 | Reverse Words in a String | Medium | [LeetCode](https://leetcode.com/problems/reverse-words-in-a-string/) | Split on whitespace, reverse words list. |

---

### Solution 1: Valid Palindrome

**Intuition:** 
Use the opposite-ends two-pointer template. 
- Skip characters that are not letters or numbers (`isalnum`).
- Convert characters to lowercase before comparing to make the check case-insensitive.

**Complexity:**
- **Time:** $O(n)$ — Single pass traversal of the string.
- **Space:** $O(1)$ — Done in-place without creating a new string.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    while (left < right) {
        // Skip non-alphanumeric characters from left
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        // Skip non-alphanumeric characters from right
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        
        // Compare lowercase characters
        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

#### Python
```python
def is_palindrome(s):
    left, right = 0, len(s) - 1
    while left < right:
        # Skip non-alphanumeric characters
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
            
        if s[left].lower() != s[right].lower():
            return False
        left += 1
        right -= 1
    return True
```

#### C++
```cpp
bool isPalindrome(string s) {
    int left = 0;
    int right = s.size() - 1;
    while (left < right) {
        while (left < right && !isalnum(s[left])) {
            left++;
        }
        while (left < right && !isalnum(s[right])) {
            right--;
        }
        if (tolower(s[left]) != tolower(s[right])) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `s = "A man, a plan, a canal: Panama"`

- `left` skips non-alphanumeric spaces/punctuation.
- Pointers check:
  - `s[left] = 'A'` vs `s[right] = 'a'` -> Match!
  - `s[left] = 'm'` vs `s[right] = 'm'` -> Match!
  - `s[left] = 'a'` vs `s[right] = 'a'` -> Match!
  - `s[left] = 'n'` vs `s[right] = 'n'` -> Match!
- Pointers eventually meet at center `'e'` (in "canal") without mismatch.

**Result:** `True` ✅
</details>

---

### Solution 2: Valid Anagram

**Intuition:** 
If lengths differ, they cannot be anagrams. Create a `26`-size integer array. For each character at index `i`, increment the count for `s[i]` and decrement for `t[i]`. If all elements in the count array are 0 at the end, they contain the same characters.

**Complexity:**
- **Time:** $O(n)$ — Single pass over both strings.
- **Space:** $O(1)$ — The frequency array size is always 26, which is constant.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] count = new int[26];
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++; // Increment for s
        count[t.charAt(i) - 'a']--; // Decrement for t
    }
    for (int val : count) {
        if (val != 0) return false;
    }
    return true;
}
```

#### Python
```python
def is_anagram(s, t):
    if len(s) != len(t):
        return False
    count = [0] * 26
    for i in range(len(s)):
        count[ord(s[i]) - ord('a')] += 1
        count[ord(t[i]) - ord('a')] -= 1
    return all(val == 0 for val in count)
```

#### C++
```cpp
bool isAnagram(string s, string t) {
    if (s.size() != t.size()) return false;
    int count[26] = {0};
    for (int i = 0; i < s.size(); i++) {
        count[s[i] - 'a']++;
        count[t[i] - 'a']--;
    }
    for (int val : count) {
        if (val != 0) return false;
    }
    return true;
}
```
</details>

---

### Solution 3: First Unique Character in a String

**Intuition:**
Use a two-pass approach:
1. Pass 1: Count occurrences of all characters and store them in our `26`-size count array.
2. Pass 2: Iterate through the string sequentially. The first character whose frequency count is `1` is the first unique character. Return its index.

**Complexity:**
- **Time:** $O(n)$ — Two passes of size $n$.
- **Space:** $O(1)$ — fixed frequency array.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public int firstUniqChar(String s) {
    int[] freq = new int[26];
    // Pass 1: Count frequencies
    for (int i = 0; i < s.length(); i++) {
        freq[s.charAt(i) - 'a']++;
    }
    // Pass 2: Find first character with count == 1
    for (int i = 0; i < s.length(); i++) {
        if (freq[s.charAt(i) - 'a'] == 1) {
            return i;
        }
    }
    return -1;
}
```

#### Python
```python
def first_uniq_char(s):
    freq = [0] * 26
    # Pass 1: Count frequencies
    for ch in s:
        freq[ord(ch) - ord('a')] += 1
    # Pass 2: Find index
    for i in range(len(s)):
        if freq[ord(s[i]) - ord('a')] == 1:
            return i
    return -1
```

#### C++
```cpp
int firstUniqChar(string s) {
    int freq[26] = {0};
    // Pass 1: Count frequencies
    for (char ch : s) {
        freq[ch - 'a']++;
    }
    // Pass 2: Find index
    for (int i = 0; i < s.size(); i++) {
        if (freq[s[i] - 'a'] == 1) {
            return i;
        }
    }
    return -1;
}
```
</details>

---

### Solution 4: Reverse String (Array of Characters)

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public void reverseString(char[] s) {
    int left = 0;
    int right = s.length - 1;
    while (left < right) {
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--;
    }
}
```

#### Python
```python
def reverse_string(s):
    left, right = 0, len(s) - 1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

#### C++
```cpp
void reverseString(vector<char>& s) {
    int left = 0;
    int right = s.size() - 1;
    while (left < right) {
        swap(s[left], s[right]);
        left++;
        right--;
    }
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `s = ["h", "e", "l", "l", "o"]`

| `left` | `right` | Swap Target | Array State | Next `left`, `right` |
| :---: | :---: | :---: | :---: | :---: |
| 0 | 4 | `s[0]` ('h') $\leftrightarrow$ `s[4]` ('o') | `["o", "e", "l", "l", "h"]` | 1, 3 |
| 1 | 3 | `s[1]` ('e') $\leftrightarrow$ `s[3]` ('l') | `["o", "l", "l", "e", "h"]` | 2, 2 |
| 2 | 2 | `left < right` (False) | Loop terminates | Finished |

**Result:** `["o", "l", "l", "e", "h"]` ✅
</details>

---

### Solution 5: Reverse Words in a String

**Intuition:** 
Split the string into individual words by checking for spaces. Reverse the order of the words list, and concatenate them back together separated by a single space.

**Complexity:**
- **Time:** $O(n)$ — Splitting and joining takes linear time.
- **Space:** $O(n)$ — To store the split list of words.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public String reverseWords(String s) {
    // Split using a regex that matches one or more spaces
    String[] words = s.trim().split("\\s+");
    StringBuilder sb = new StringBuilder();
    // Append words in reverse order
    for (int i = words.length - 1; i >= 0; i--) {
        sb.append(words[i]);
        if (i > 0) {
            sb.append(" ");
        }
    }
    return sb.toString();
}
```

#### Python
```python
def reverse_words(s):
    # s.split() automatically handles multiple spaces and trims
    words = s.split()
    # Reverse and join back with space
    return " ".join(reversed(words))
```

#### C++
```cpp
string reverseWords(string s) {
    stringstream ss(s);
    string word;
    vector<string> words;
    // stringstream >> skips whitespace automatically
    while (ss >> word) {
        words.push_back(word);
    }
    string result = "";
    for (int i = words.size() - 1; i >= 0; i--) {
        result += words[i];
        if (i > 0) {
            result += " ";
        }
    }
    return result;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `s = "  the sky is  blue  "`

1. `trim()` / `split()` extracts words array: `words = ["the", "sky", "is", "blue"]`
2. Reverse iteration:
   - `i = 3`: Append `words[3]` ("blue") + `" "` $\rightarrow$ `"blue "`
   - `i = 2`: Append `words[2]` ("is") + `" "` $\rightarrow$ `"blue is "`
   - `i = 1`: Append `words[1]` ("sky") + `" "` $\rightarrow$ `"blue is sky "`
   - `i = 0`: Append `words[0]` ("the") $\rightarrow$ `"blue is sky the"`

**Result:** `"blue is sky the"` ✅
</details>

---

## 🎓 Viva Questions & Answers

### Q1: What does String Immutability mean, and why are Strings immutable in Java and Python?
**Answer:**
String immutability means once a `String` object is created in memory, its content cannot be altered. If you modify a string, a brand-new String object is created in heap memory.
**Why?**
1. **Security:** Strings carry sensitive network URLs, passwords, and file paths.
2. **Thread Safety:** Immutable objects are inherently thread-safe.
3. **String Constant Pool:** Allows caching and sharing duplicate string literals to save memory.

### Q2: What is the difference between `String`, `StringBuilder`, and `StringBuffer` in Java?
**Answer:**
- **`String`:** Immutable, thread-safe, slow for heavy string concatenation ($O(n^2)$ due to repeated copies).
- **`StringBuilder`:** Mutable, non-thread-safe, fast $O(1)$ amortized character appends.
- **`StringBuffer`:** Mutable, thread-safe (synchronized methods), slightly slower than `StringBuilder`.

### Q3: How does `c - 'a'` convert a lowercase character to a 0-indexed integer?
**Answer:**
Characters are represented by their ASCII numeric code (`'a' = 97`, `'b' = 98`, ..., `'z' = 122`).
Subtracting `'a'` (97) offsets the character code to a 0–25 range:
- `'a' - 'a' = 97 - 97 = 0`
- `'c' - 'a' = 99 - 97 = 2`

### Q4: Why is string concatenation inside a loop bad practice?
**Answer:**
In languages with immutable strings (Java/Python), `res = res + str[i]` creates a new string copy of length $k$ at iteration $i$. Over $n$ loop iterations, total work is $1 + 2 + \dots + n = O(n^2)$ time. Using `StringBuilder` / `list.append()` + `.join()` reduces time to $O(n)$.

### Q5: What is the KMP (Knuth-Morris-Pratt) algorithm, and what problem does it solve?
**Answer:**
KMP is a pattern-searching algorithm that finds occurrences of a pattern $P$ of length $m$ inside text $T$ of length $n$ in $O(n + m)$ time. It uses a **Longest Prefix Suffix (LPS)** array to avoid re-checking characters that have already matched.

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Comparing Strings with `==` in Java:**
   - In Java, doing `str1 == str2` compares the memory addresses of the objects, not their characters. Always use `.equals()`: `if (str1.equals(str2))`. (In C++ and Python, `==` behaves as expected, comparing contents).

2. **Index out of bounds on `c - 'a'`:**
   - Make sure your input only contains lowercase alphabetic characters when doing `c - 'a'`. If the string can contain uppercase letters or symbols, this calculation will result in negative indices or indices $> 25$, crashing your program. Use a general `HashMap` or a larger `int[128]` / `int[256]` array to count all standard ASCII characters.

---

> 👉 Next, open `06-Recursion.md` to explore the beauty of recursive call stacks! 💪

