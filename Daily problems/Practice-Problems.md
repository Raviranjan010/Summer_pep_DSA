# 🎯 DSA Practice Problems — Pattern Wise (Batch 2026)

> Your complete placement problem list, organized by pattern. **Solve top to bottom within each section** — they're ordered so each problem builds on the last. — *Ajai Raj (Mentor)*

---

## 📊 Quick Stats

| Topic | Count |
|-------|-------|
| Binary Search | 13 |
| Two Pointers | 5 |
| Maps / Hashing | 7 |
| Prefix Sum | 6 |
| Sliding Window (Fixed + Variable) | 18 |
| Prefix Sum + Map | 3 |
| Counting Subarrays | 3 |
| Recursion + Backtracking | 20 |
| Linked List | 17 |
| Stack (Fundamentals + Simulation) | 8 |
| Stack (Advanced) + Queues | 14 |
| Trees | 16 |
| Heaps | 9 |
| BST | 10 |
| Graphs (BFS/DFS) | 12 |
| Graphs (Multi-Source BFS) | 4 |
| Graphs (Topo Sort) | 5 |
| Graphs (Dijkstra) | 5 |
| Graphs (DSU) | 10 |
| DP (1D) | 9 |
| DP (2D) | 6 |
| DP (0/1 Knapsack) | 8 |
| DP (Unbounded Knapsack) | 4 |
| **TOTAL** | **~202** |

---

## 🔍 Binary Search

### Standard Binary Search

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Binary Search | Easy | https://leetcode.com/problems/binary-search/ | The core skeleton — memorize this |
| Find First and Last Position of Element in Sorted Array | Medium | https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/ | Two biased searches (left / right bound) |
| Floor in a Sorted Array | Easy | https://www.geeksforgeeks.org/problems/floor-in-a-sorted-array-1587115620/1 | Largest element ≤ target |
| Ceil the Floor | Easy | https://www.geeksforgeeks.org/problems/ceil-the-floor2802/1 | Smallest element ≥ target |

### Rotated Sorted Array

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Search in Rotated Sorted Array | Medium | https://leetcode.com/problems/search-in-rotated-sorted-array/ | One half is always sorted — check which |
| Search in Rotated Sorted Array II | Medium | https://leetcode.com/problems/search-in-rotated-sorted-array-ii/ | Handle duplicates by shrinking bounds |
| Find Minimum in Rotated Sorted Array | Medium | https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/ | Compare mid to right end to find break |
| Find Peak Element | Medium | https://leetcode.com/problems/find-peak-element/ | Climb toward the uphill neighbor |

### Binary Search on Answers

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Sqrt(x) | Easy | https://leetcode.com/problems/sqrtx/ | Search the ANSWER space, not the array |
| Koko Eating Bananas | Medium | https://leetcode.com/problems/koko-eating-bananas/ | Binary search on eating speed |
| Capacity To Ship Packages Within D Days | Medium | https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/ | Binary search on ship capacity |
| Minimum Number of Days to Make m Bouquets | Medium | https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/ | Binary search on number of days |
| Magnetic Force Between Two Balls | Medium | https://leetcode.com/problems/magnetic-force-between-two-balls/ | Binary search on minimum gap |
| Minimize Max Distance to Gas Station | Hard | https://www.geeksforgeeks.org/problems/minimize-max-distance-to-gas-station/1 | Binary search on a decimal answer |

---

## 🤝 Two Pointers

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Squares of a Sorted Array | Easy | https://leetcode.com/problems/squares-of-a-sorted-array/ | Compare both ends, fill from the back |
| Two Sum II – Input Array Is Sorted | Medium | https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/ | Left/right pointers, shrink based on sum |
| 3Sum | Medium | https://leetcode.com/problems/3sum/ | Fix one, two-pointer the rest |
| 4Sum | Medium | https://leetcode.com/problems/4sum/ | Fix two, two-pointer the rest |
| Number of Subsequences That Satisfy the Given Sum Condition | Medium | https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/ | Sort + two pointers + power of 2 |

---

## 🗃️ Maps / Hashing

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Contains Duplicate | Easy | https://leetcode.com/problems/contains-duplicate/ | HashSet — seen before? |
| Valid Anagram | Easy | https://leetcode.com/problems/valid-anagram/ | Frequency array, add & subtract |
| First Unique Character in a String | Easy | https://leetcode.com/problems/first-unique-character-in-a-string/ | Frequency array, 2 passes |
| Find the Difference | Easy | https://leetcode.com/problems/find-the-difference/ | Frequency count / XOR |
| Isomorphic Strings | Easy | https://leetcode.com/problems/isomorphic-strings/ | Two maps, both directions |
| Find Common Characters | Easy | https://leetcode.com/problems/find-common-characters/ | Minimum frequency across words |
| Sort Characters by Frequency | Medium | https://leetcode.com/problems/sort-characters-by-frequency/ | Count + sort by frequency |

---

## ➕ Prefix Sum

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Running Sum of 1D Array | Easy | https://leetcode.com/problems/running-sum-of-1d-array/ | Build the prefix sum |
| Find Pivot Index | Easy | https://leetcode.com/problems/find-pivot-index/ | left sum == right sum using total |
| Left and Right Sum Differences | Easy | https://leetcode.com/problems/left-and-right-sum-differences/ | Prefix + abs(left − right) |
| Minimum Value to Get Positive Step by Step Sum | Easy | https://leetcode.com/problems/minimum-value-to-get-positive-step-by-step-sum/ | Prefix + track the minimum |
| Find the Highest Altitude | Easy | https://leetcode.com/problems/find-the-highest-altitude/ | Prefix + track the maximum |
| Product of Array Except Self | Medium | https://leetcode.com/problems/product-of-array-except-self/ | Left product × right product |

---

## 🪟 Sliding Window — Fixed Size

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Maximum Average Subarray I | Easy | https://leetcode.com/problems/maximum-average-subarray-i/ | Fixed window sum → max |
| Maximum Number of Vowels in a Substring | Medium | https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/ | Fixed window count → max |
| Permutation in String | Medium | https://leetcode.com/problems/permutation-in-string/ | Fixed window + frequency match |
| Sub-arrays of Size K and Avg ≥ Threshold | Medium | https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/ | Fixed window sum → count valid |

## 🪟 Sliding Window — Variable Size

### Standard

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Minimum Size Subarray Sum | Medium | https://leetcode.com/problems/minimum-size-subarray-sum/ | Shrink to find shortest valid window |
| Max Consecutive Ones III | Medium | https://leetcode.com/problems/max-consecutive-ones-iii/ | Shrink when zeros > k |

### Set + Sliding Window

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Longest Substring Without Repeating Characters | Medium | https://leetcode.com/problems/longest-substring-without-repeating-characters/ | Set for uniqueness, shrink on repeat |
| Longest Substring with Distinct Characters | Medium | https://www.geeksforgeeks.org/problems/longest-distinct-characters-in-string5848/1 | Same as LC 3 |
| Maximum Erasure Value | Medium | https://leetcode.com/problems/maximum-erasure-value/ | Set + running sum in window |

### Map + Sliding Window

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Longest K Unique Characters Substring | Medium | https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1 | Map + shrink when types > k |
| Fruit Into Baskets | Medium | https://leetcode.com/problems/fruit-into-baskets/ | Map + at most 2 types |

### Prefix Sum + Map

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Subarray Sum Equals K | Medium | https://leetcode.com/problems/subarray-sum-equals-k/ | Prefix sum + hashmap of counts |
| Subarray Sums Divisible by K | Medium | https://leetcode.com/problems/subarray-sums-divisible-by-k/ | Prefix mod + map count |
| Continuous Subarray Sum | Medium | https://leetcode.com/problems/continuous-subarray-sum/ | Prefix mod k + first-seen index |

### Counting Subarrays

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Count Number of Nice Subarrays | Medium | https://leetcode.com/problems/count-number-of-nice-subarrays/ | atMost(k) − atMost(k−1) |
| Binary Subarrays With Sum | Medium | https://leetcode.com/problems/binary-subarrays-with-sum/ | atMost(k) − atMost(k−1) |
| Subarray Product Less Than K | Medium | https://leetcode.com/problems/subarray-product-less-than-k/ | Variable window, shrink when product ≥ k |

---

## 🔁 Recursion + Backtracking

### Basics

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Print 1 to N Without Loops | Easy | https://www.geeksforgeeks.org/problems/print-1-to-n-without-using-loops-1587115620/1 | Print after call (way up) |
| Print N to 1 Without Loops | Easy | https://www.geeksforgeeks.org/problems/print-n-to-1-without-loop/1 | Print before call (way down) |
| Sum of Digits | Easy | https://www.geeksforgeeks.org/problems/sum-of-digits1742/1 | n%10 + recurse(n/10) |
| Pow(x, n) | Medium | https://leetcode.com/problems/powx-n/ | Fast power: x^n = (x^(n/2))² |

### Recursion on Arrays & Strings

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Sum of Array Using Recursion | Easy | https://www.geeksforgeeks.org/problems/sum-of-array2326/1 | arr[0] + recurse(rest) |
| Reverse String | Easy | https://leetcode.com/problems/reverse-string/ | Two pointers recursively |
| Binary Search (Recursive) | Easy | https://leetcode.com/problems/binary-search/ | Divide & conquer |

### Multiple Recursive Calls

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Fibonacci Number | Easy | https://leetcode.com/problems/fibonacci-number/ | Two calls → tree recursion |
| Tower of Hanoi | Medium | https://www.geeksforgeeks.org/problems/tower-of-hanoi-1587115621/1 | Move n−1, move 1, move n−1 |
| K-th Symbol in Grammar | Medium | https://leetcode.com/problems/k-th-symbol-in-grammar/ | Parent row determines child |

### Pick / Not Pick (Intro to Backtracking)

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Subsets | Medium | https://leetcode.com/problems/subsets/ | Pick/not-pick for each element |
| Subset Sums | Medium | https://www.geeksforgeeks.org/problems/subset-sums2234/1 | Pick/not-pick, collect sum at leaf |
| Subsets II | Medium | https://leetcode.com/problems/subsets-ii/ | Sort + skip duplicates |

### Combinations & Target-Based Backtracking

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Combinations | Medium | https://leetcode.com/problems/combinations/ | Choose k from n |
| Combination Sum | Medium | https://leetcode.com/problems/combination-sum/ | Unlimited picks, target shrinks |
| Combination Sum II | Medium | https://leetcode.com/problems/combination-sum-ii/ | Each used once + skip duplicates |
| Generate Parentheses | Medium | https://leetcode.com/problems/generate-parentheses/ | open < n → '(', close < open → ')' |

### Permutation Pattern

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Letter Case Permutation | Medium | https://leetcode.com/problems/letter-case-permutation/ | At each letter: lower or upper |
| Letter Combinations of a Phone Number | Medium | https://leetcode.com/problems/letter-combinations-of-a-phone-number/ | Each digit maps to letters |
| Permutations | Medium | https://leetcode.com/problems/permutations/ | Swap-based or used-array |
| Permutations II | Medium | https://leetcode.com/problems/permutations-ii/ | Sort + skip same-level duplicates |
| Palindrome Partitioning | Medium | https://leetcode.com/problems/palindrome-partitioning/ | Partition at every index, check palindrome |

### Grid & Constraint Backtracking

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Rat in a Maze | Medium | https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1 | Try all 4 directions, backtrack |
| Word Search | Medium | https://leetcode.com/problems/word-search/ | DFS on grid + visited tracking |
| N-Queens | Hard | https://leetcode.com/problems/n-queens/ | Place row by row, check col/diag |
| Sudoku Solver | Hard | https://leetcode.com/problems/sudoku-solver/ | Try 1–9 in empty cells, validate |

---

## 🔗 Linked List

### Basics & Fundamentals

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Implement Linked List | Easy | https://www.geeksforgeeks.org/problems/introduction-to-linked-list/1 | Node + next pointer |
| Traverse Linked List | Easy | https://www.geeksforgeeks.org/problems/count-nodes-of-linked-list/1 | curr = curr.next |
| Find Length of Linked List | Easy | https://www.geeksforgeeks.org/problems/count-nodes-of-linked-list/1 | count++ while walking |
| Search in Linked List | Easy | https://www.geeksforgeeks.org/problems/search-in-linked-list-1664434326/1 | Linear search walk |

### Singly Linked List

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Reverse Linked List | Easy | https://leetcode.com/problems/reverse-linked-list/ | prev / curr / next |
| Middle of the Linked List | Easy | https://leetcode.com/problems/middle-of-the-linked-list/ | slow / fast |
| Remove Nth Node From End | Medium | https://leetcode.com/problems/remove-nth-node-from-end-of-list/ | Gap technique + dummy |
| Merge Two Sorted Lists | Easy | https://leetcode.com/problems/merge-two-sorted-lists/ | Dummy + compare & attach |
| Intersection of Two Linked Lists | Easy | https://leetcode.com/problems/intersection-of-two-linked-lists/ | Two-pass or length diff |
| Odd Even Linked List | Medium | https://leetcode.com/problems/odd-even-linked-list/ | Separate chains + join |
| Remove Duplicates from Sorted List | Easy | https://leetcode.com/problems/remove-duplicates-from-sorted-list/ | Skip if curr.val == next.val |
| Add Two Numbers | Medium | https://leetcode.com/problems/add-two-numbers/ | Carry math: sum%10, sum/10 |
| Rotate List | Medium | https://leetcode.com/problems/rotate-list/ | Length + connect + break |
| Palindrome Linked List | Easy | https://leetcode.com/problems/palindrome-linked-list/ | Middle + reverse + compare |

### Cyclic Pattern

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Linked List Cycle | Easy | https://leetcode.com/problems/linked-list-cycle/ | Floyd's — slow & fast meet |
| Linked List Cycle II | Medium | https://leetcode.com/problems/linked-list-cycle-ii/ | Floyd + reset to head |
| Length of Loop in LL | Easy | https://www.geeksforgeeks.org/problems/find-length-of-loop/1 | After meeting, count loop length |

---

## 🥞 Stack — Fundamentals + Simulation

### Implementation

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Implement Stack Using Array | Easy | https://www.geeksforgeeks.org/problems/implement-stack-using-array/1 | Array + top pointer |
| Implement Stack Using Linked List | Easy | https://www.geeksforgeeks.org/problems/implement-stack-using-linked-list/1 | Push/pop at head |
| Implement Stack Using Queues | Easy | https://leetcode.com/problems/implement-stack-using-queues/ | Pop = transfer n−1, dequeue last |

### Basic Simulation

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Baseball Game | Easy | https://leetcode.com/problems/baseball-game/ | Stack follows operation rules |
| Remove Outermost Parentheses | Easy | https://leetcode.com/problems/remove-outermost-parentheses/ | Counter tracks depth, skip outer |
| Backspace String Compare | Easy | https://leetcode.com/problems/backspace-string-compare/ | '#' = pop, then compare |
| Make The String Great | Easy | https://leetcode.com/problems/make-the-string-great/ | Stack top and curr differ by 32 → pop |
| Remove All Adjacent Duplicates in String | Easy | https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/ | Push if different, pop if same as top |

---

## 🥞 Stack (Advanced) + Queues

### Brackets Pattern

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Valid Parentheses | Easy | https://leetcode.com/problems/valid-parentheses/ | Push opening, match & pop |
| Minimum Add to Make Parentheses Valid | Medium | https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/ | Count unmatched |
| Minimum Remove to Make Valid Parentheses | Medium | https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/ | Push indices, remove unmatched |
| Longest Valid Parentheses | Hard | https://leetcode.com/problems/longest-valid-parentheses/ | Stack of indices, track distance |

### Next Greater/Smaller Element (Monotonic Stack)

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Next Greater Element | Medium | https://www.geeksforgeeks.org/problems/next-larger-element-1587115620/1 | Stack keeps decreasing order |
| Next Greater Element I | Easy | https://leetcode.com/problems/next-greater-element-i/ | Monotonic stack + hashmap |
| Next Greater Element II | Medium | https://leetcode.com/problems/next-greater-element-ii/ | Circular → traverse 2× |
| Daily Temperatures | Medium | https://leetcode.com/problems/daily-temperatures/ | Stack of indices, pop when warmer |
| Online Stock Span | Medium | https://leetcode.com/problems/online-stock-span/ | Stack of (price, span) pairs |

### Expression Evaluation

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Evaluate Reverse Polish Notation | Medium | https://leetcode.com/problems/evaluate-reverse-polish-notation/ | Stack of operands, pop 2 on operator |
| Decode String | Medium | https://leetcode.com/problems/decode-string/ | Two stacks: counts + strings |
| Simplify Path | Medium | https://leetcode.com/problems/simplify-path/ | Stack of directories, '..' = pop |
| Basic Calculator II | Medium | https://leetcode.com/problems/basic-calculator-ii/ | Stack + handle precedence |
| Basic Calculator | Hard | https://leetcode.com/problems/basic-calculator/ | Stack for nested parentheses |

### Design Questions

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Min Stack | Medium | https://leetcode.com/problems/min-stack/ | Two stacks or store (val, min) pairs |
| Design a Stack With Increment | Medium | https://leetcode.com/problems/design-a-stack-with-increment-operation/ | Lazy increment with array |
| Maximum Frequency Stack | Hard | https://leetcode.com/problems/maximum-frequency-stack/ | Map of freq → stack |

---

## 🌳 Trees

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Binary Tree Preorder Traversal | Easy | https://leetcode.com/problems/binary-tree-preorder-traversal/ | Root → Left → Right |
| Binary Tree Level Order Traversal | Medium | https://leetcode.com/problems/binary-tree-level-order-traversal/ | BFS with queue |
| Maximum Depth of Binary Tree | Easy | https://leetcode.com/problems/maximum-depth-of-binary-tree/ | 1 + max(left, right) |
| Same Tree | Easy | https://leetcode.com/problems/same-tree/ | Compare nodes recursively |
| Symmetric Tree | Easy | https://leetcode.com/problems/symmetric-tree/ | Mirror check |
| Balanced Binary Tree | Easy | https://leetcode.com/problems/balanced-binary-tree/ | Height diff ≤ 1 at every node |
| Diameter of Binary Tree | Easy | https://leetcode.com/problems/diameter-of-binary-tree/ | leftH + rightH at each node |
| Path Sum | Easy | https://leetcode.com/problems/path-sum/ | Subtract node val, check leaf |
| Binary Tree Right Side View | Medium | https://leetcode.com/problems/binary-tree-right-side-view/ | Level order, take last per level |
| Lowest Common Ancestor | Medium | https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/ | Node where left & right both find target |
| Convert Sorted Array to BST | Easy | https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/ | Mid as root, recurse halves |
| Insert into a BST | Medium | https://leetcode.com/problems/insert-into-a-binary-search-tree/ | Walk left/right, attach at null |
| Construct BST from Preorder | Medium | https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/ | Upper bound approach |
| Construct BT from Preorder & Inorder | Medium | https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/ | Preorder[0] = root, split inorder |
| Construct BT from Inorder & Postorder | Medium | https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/ | Postorder[last] = root, split inorder |
| Maximum Binary Tree | Medium | https://leetcode.com/problems/maximum-binary-tree/ | Max as root, recurse left/right |

---

## ⛰️ Heaps

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Kth Largest Element in an Array | Medium | https://leetcode.com/problems/kth-largest-element-in-an-array/ | Min-heap of size k |
| Last Stone Weight | Easy | https://leetcode.com/problems/last-stone-weight/ | Max-heap, smash two biggest |
| Kth Largest Element in a Stream | Easy | https://leetcode.com/problems/kth-largest-element-in-a-stream/ | Min-heap of size k, peek = answer |
| Relative Ranks | Easy | https://leetcode.com/problems/relative-ranks/ | Sort + map index |
| Top K Frequent Elements | Medium | https://leetcode.com/problems/top-k-frequent-elements/ | Count map + min-heap of size k |
| K Closest Points to Origin | Medium | https://leetcode.com/problems/k-closest-points-to-origin/ | Max-heap of size k by distance |
| Sort a Nearly Sorted Array | Medium | https://www.geeksforgeeks.org/problems/nearly-sorted-1587115620/1 | Min-heap of size k+1 |
| Merge K Sorted Lists | Hard | https://leetcode.com/problems/merge-k-sorted-lists/ | Min-heap of k list heads |
| Reorganize String | Medium | https://leetcode.com/problems/reorganize-string/ | Max-heap by freq, alternate chars |

---

## 🌲 BST (Binary Search Tree)

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Search in a BST | Easy | https://leetcode.com/problems/search-in-a-binary-search-tree/ | Go left if smaller, right if bigger |
| Insert into a BST | Medium | https://leetcode.com/problems/insert-into-a-binary-search-tree/ | Walk to null, attach |
| Minimum Value in BST | Easy | https://www.geeksforgeeks.org/problems/minimum-element-in-bst/1 | Go all the way left |
| Validate BST | Medium | https://leetcode.com/problems/validate-binary-search-tree/ | Pass min/max range down |
| Lowest Common Ancestor of a BST | Medium | https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/ | Split point where p & q diverge |
| Kth Smallest Element in a BST | Medium | https://leetcode.com/problems/kth-smallest-element-in-a-bst/ | Inorder traversal = sorted |
| Minimum Absolute Difference in BST | Easy | https://leetcode.com/problems/minimum-absolute-difference-in-bst/ | Inorder, compare adjacent |
| Two Sum IV – Input is a BST | Easy | https://leetcode.com/problems/two-sum-iv-input-is-a-bst/ | Inorder → sorted → two pointers |
| Delete Node in a BST | Medium | https://leetcode.com/problems/delete-node-in-a-bst/ | Replace with inorder successor |
| Construct BST from Preorder | Medium | https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/ | Upper bound approach |

---

## 🗺️ Graphs — BFS / DFS

### On Adjacency List

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| BFS of Graph | Easy | https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1 | Queue + visited |
| Find if Path Exists in Graph | Easy | https://leetcode.com/problems/find-if-path-exists-in-graph/ | BFS/DFS from source |
| Keys and Rooms | Medium | https://leetcode.com/problems/keys-and-rooms/ | DFS, each room gives keys |
| Number of Provinces | Medium | https://leetcode.com/problems/number-of-provinces/ | Count connected components |

### On Grid

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Number of Islands | Medium | https://leetcode.com/problems/number-of-islands/ | DFS/BFS from each '1', mark visited |
| Flood Fill | Easy | https://leetcode.com/problems/flood-fill/ | DFS from start, change color |
| Surrounded Regions | Medium | https://leetcode.com/problems/surrounded-regions/ | DFS from border O's, flip the rest |
| Number of Closed Islands | Medium | https://leetcode.com/problems/number-of-closed-islands/ | Mark border-connected, count rest |
| Island Perimeter | Easy | https://leetcode.com/problems/island-perimeter/ | Count edges touching water/boundary |
| Pacific Atlantic Water Flow | Medium | https://leetcode.com/problems/pacific-atlantic-water-flow/ | BFS from both oceans, intersection |
| Count Sub Islands | Medium | https://leetcode.com/problems/count-sub-islands/ | Island in grid2 ⊂ grid1 |

---

## 🗺️ Graphs — Multi-Source BFS

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Rotting Oranges | Medium | https://leetcode.com/problems/rotting-oranges/ | All rotten start in queue, BFS waves |
| 01 Matrix | Medium | https://leetcode.com/problems/01-matrix/ | All 0s start in queue, BFS outward |
| As Far from Land as Possible | Medium | https://leetcode.com/problems/as-far-from-land-as-possible/ | All land in queue, BFS to water |
| Map of Highest Peak | Medium | https://leetcode.com/problems/map-of-highest-peak/ | All water start, BFS outward |

---

## 🗺️ Graphs — Topological Sort

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Course Schedule | Medium | https://leetcode.com/problems/course-schedule/ | Cycle detection via topo sort |
| Course Schedule II | Medium | https://leetcode.com/problems/course-schedule-ii/ | Topo sort → valid order |
| Prerequisite Tasks | Medium | https://www.geeksforgeeks.org/problems/prerequisite-tasks/1 | Same as Course Schedule I |
| Detect Cycle in Directed Graph (BFS) | Medium | https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1 | Kahn's — processed < total → cycle |
| Find Eventual Safe States | Medium | https://leetcode.com/problems/find-eventual-safe-states/ | Reverse graph + topo sort |

---

## 🗺️ Graphs — Dijkstra

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Dijkstra Algorithm | Medium | https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1 | Min-heap + relaxation |
| Network Delay Time | Medium | https://leetcode.com/problems/network-delay-time/ | Dijkstra from source, max of dists |
| Minimum Cost Path | Medium | https://www.geeksforgeeks.org/problems/minimum-cost-path3833/1 | Dijkstra on grid |
| Path With Minimum Effort | Medium | https://leetcode.com/problems/path-with-minimum-effort/ | Dijkstra, weight = height diff |
| Cheapest Flights Within K Stops | Medium | https://leetcode.com/problems/cheapest-flights-within-k-stops/ | BFS with level limit |

---

## 🗺️ Graphs — DSU (Disjoint Set Union)

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Disjoint Set Union | Easy | https://www.geeksforgeeks.org/problems/disjoint-set-union-find/1 | Union by rank + path compression |
| Detect Cycle using DSU | Medium | https://www.geeksforgeeks.org/problems/detect-cycle-using-dsu/1 | Same parent = cycle |
| Number of Operations to Make Network Connected | Medium | https://leetcode.com/problems/number-of-operations-to-make-network-connected/ | Components − 1 = needed edges |
| Accounts Merge | Medium | https://leetcode.com/problems/accounts-merge/ | Union emails, group by root |
| Redundant Connection | Medium | https://leetcode.com/problems/redundant-connection/ | Edge causing cycle = answer |
| Most Stones Removed | Medium | https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/ | n − components = removable |
| Making a Large Island | Hard | https://leetcode.com/problems/making-a-large-island/ | DSU islands, try flipping each 0 |
| Number of Islands II | Hard | https://www.geeksforgeeks.org/problems/number-of-islands/1 | Online DSU as land is added |
| Kruskal's Algorithm | Medium | https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1 | Sort edges + DSU |
| Satisfiability of Equality Equations | Medium | https://leetcode.com/problems/satisfiability-of-equality-equations/ | Union '==' pairs, check '!=' |

---

## 📈 DP — 1D

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Fibonacci Number | Easy | https://leetcode.com/problems/fibonacci-number/ | dp[i] = dp[i−1] + dp[i−2] |
| Friends Pairing Problem | Easy | https://www.geeksforgeeks.org/problems/friends-pairing-problem5425/1 | Single + pair with each remaining |
| Climbing Stairs | Easy | https://leetcode.com/problems/climbing-stairs/ | Same as Fibonacci |
| Frog Jump | Medium | https://www.geeksforgeeks.org/problems/geek-jump/1 | Min cost via 1 or 2 steps |
| House Robber | Medium | https://leetcode.com/problems/house-robber/ | max(take+dp[i−2], dp[i−1]) |
| House Robber II | Medium | https://leetcode.com/problems/house-robber-ii/ | Circular: run twice, skip first or last |
| Tribonacci Number | Easy | https://leetcode.com/problems/n-th-tribonacci-number/ | dp[i] = dp[i−1]+dp[i−2]+dp[i−3] |
| Min Cost Climbing Stairs | Easy | https://leetcode.com/problems/min-cost-climbing-stairs/ | cost[i] + min(dp[i−1], dp[i−2]) |
| Delete and Earn | Medium | https://leetcode.com/problems/delete-and-earn/ | Reduce to House Robber on values |

---

## 📈 DP — 2D

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Unique Paths | Medium | https://leetcode.com/problems/unique-paths/ | dp[i][j] = dp[i−1][j] + dp[i][j−1] |
| Unique Paths II | Medium | https://leetcode.com/problems/unique-paths-ii/ | Same + skip obstacles |
| Minimum Path Sum | Medium | https://leetcode.com/problems/minimum-path-sum/ | dp = grid val + min(up, left) |
| Triangle | Medium | https://leetcode.com/problems/triangle/ | Bottom-up, pick min below |
| Dungeon Game | Hard | https://leetcode.com/problems/dungeon-game/ | Bottom-right to top-left, need ≥ 1 |
| Minimum Falling Path Sum | Medium | https://leetcode.com/problems/minimum-falling-path-sum/ | dp from row above, 3 neighbors |

---

## 📈 DP — 0/1 Knapsack

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| 0/1 Knapsack | Medium | https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1 | Take or skip each item |
| Subset Sum Problem | Medium | https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1 | Can we reach this sum? |
| Partition Equal Subset Sum | Medium | https://leetcode.com/problems/partition-equal-subset-sum/ | Subset sum = totalSum / 2 |
| Count of Subset Sum | Medium | https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1 | Count ways to reach target |
| Minimum Subset Sum Difference | Medium | https://www.geeksforgeeks.org/problems/minimum-sum-partition3317/1 | Find reachable sums, min diff |
| Count Subsets with Given Difference | Medium | https://www.geeksforgeeks.org/problems/partitions-with-given-difference/1 | Reduce to count-of-subset-sum |
| Target Sum | Medium | https://leetcode.com/problems/target-sum/ | S1 − S2 = target → subset sum |
| Ones and Zeroes | Medium | https://leetcode.com/problems/ones-and-zeroes/ | 2D knapsack on 0s and 1s |

---

## 📈 DP — Unbounded Knapsack

| Problem | Difficulty | Link | Key Idea |
|---------|-----------|------|----------|
| Rod Cutting | Medium | https://www.geeksforgeeks.org/problems/rod-cutting0840/1 | Unlimited cuts, maximize value |
| Coin Change | Medium | https://leetcode.com/problems/coin-change/ | Min coins to reach amount |
| Coin Change II | Medium | https://leetcode.com/problems/coin-change-ii/ | Count ways to reach amount |
| Integer Break | Medium | https://leetcode.com/problems/integer-break/ | Max product of parts |

---

> 🌟 **~202 problems. Every placement pattern covered.** Solve section by section, top to bottom. The order within each section goes from foundation → interview level. You've got this. 💪 — *Ajai Raj (Mentor)*
