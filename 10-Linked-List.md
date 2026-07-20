# 🔗 10 — Linked List

> **Explain Like I'm 5:** Think of a train. Each train coach (called a **Node**) carries some cargo (the value) and is coupled to the **next** coach via a link. You cannot jump directly to coach 5 — you must walk from the engine, coach by coach. That's a Linked List!
>
> ```
> [ 10 | • ] ──► [ 20 | • ] ──► [ 30 | • ] ──► NULL (end of train)
>   val next       val next       val next
>  (head)
> ```

---

## 🧠 Memory Layout: Arrays vs. Linked Lists

To understand why we need Linked Lists, let's compare how they reside in RAM compared to Arrays:

| Feature | Arrays | Linked Lists |
| :--- | :--- | :--- |
| **Memory Allocation** | Contiguous (one single block in RAM) | Scattered (individual blocks in Heap memory) |
| **Random Access** | $O(1)$ (Using index offset arithmetic) | $O(n)$ (Must traverse element by element) |
| **Insert / Delete at End** | $O(1)$ amortized | $O(n)$ (without tail pointer) or $O(1)$ (with tail pointer) |
| **Insert / Delete at Start**| $O(n)$ (Requires shifting all elements) | $O(1)$ (Only swap a few pointers) |
| **Cache Locality** | Highly cache-friendly (spatial locality) | Poor cache-friendliness (scattered references) |

### Visualizing Linked List Memory Scattering
In a Linked List, nodes are allocated anywhere in Heap memory. They contain a pointer/reference variable storing the address of the next node:

```
HEAP ADDRESSES:

Address 0x1048: Node [ Val: 10 | Next: 0x2090 ] ─────┐
                                                      │
Address 0x10B4: Node [ Val: 30 | Next: NULL   ] ◄─────┼─────┐
                                                      │     │
Address 0x2090: Node [ Val: 20 | Next: 0x10B4 ] ◄─────┘     │
                                                            │
Logical Flow:   Head (0x1048) ──► Node (0x2090) ──► Node (0x10B4) ──► NULL
```

---

## 🛠️ The Node Class Definition

Here is how a Node is defined in Java, Python, and C++:

<details>
<summary>💻 Node Class Implementations</summary>

### Java
```java
public class ListNode {
    public int val;
    public ListNode next;
    
    public ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```

### Python
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

### C++
```cpp
struct ListNode {
    int val;
    ListNode* next;
    
    ListNode(int x) : val(x), next(nullptr) {}
};
```
</details>

---

## 📋 The Interview Pattern Cheat-Sheet

| Pattern | Description | Key Mechanism |
| :--- | :--- | :--- |
| **Traversal** | Move sequentially through the list. | `curr = curr.next` |
| **Fast & Slow** | Pointers moving at different speeds. | `slow = slow.next`, `fast = fast.next.next` |
| **Pointer Flip** | Reversing node linkages. | `next = curr.next; curr.next = prev; prev = curr; curr = next;` |
| **Dummy Node** | A fake node pointing to the head. | Eliminates head deletion/insertion edge cases. |

---

## 🟢 Day 1: Foundational Pointers

These problems form the building blocks of all linked list manipulations.

### Solution 4: Middle of the Linked List

**Intuition:** 
Use a slow pointer and a fast pointer.
- `slow` moves 1 step at a time.
- `fast` moves 2 steps at a time.
When `fast` reaches the end (`null` or `fast.next` is `null`), `slow` will be exactly at the middle node.

**Complexity:**
- **Time:** $O(n)$ — Single pass traversal.
- **Space:** $O(1)$ — Two pointer variables.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode middleNode(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;        // 1 step
        fast = fast.next.next;   // 2 steps
    }
    return slow;
}
```

#### Python
```python
def middleNode(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next        # 1 step
        fast = fast.next.next   # 2 steps
    return slow
```

#### C++
```cpp
ListNode* middleNode(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;       // 1 step
        fast = fast->next->next; // 2 steps
    }
    return slow;
}
```
</details>

---

### Solution 5: Reverse Linked List

**Intuition:**
We reverse the linkages of the nodes in-place. We need three pointers:
- `prev`: The node behind our current pointer (starts as `null`).
- `curr`: Our current pointer (starts as `head`).
- `next`: Temporarily stores the node ahead of us before we flip pointers.

```
Execution Loop (Save -> Flip -> Move -> Move):
1. next = curr.next   (Save next node)
2. curr.next = prev   (Flip pointer backward)
3. prev = curr        (Move prev forward)
4. curr = next        (Move curr forward)
```

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next; // 1. Save what's ahead
        curr.next = prev;          // 2. Flip connection backward
        prev = curr;               // 3. Move prev up
        curr = next;               // 4. Move curr up
    }
    return prev; // prev ends up pointing to the new head
}
```

#### Python
```python
def reverseList(head):
    prev = None
    curr = head
    while curr:
        nxt = curr.next   # 1. Save what's ahead
        curr.next = prev  # 2. Flip connection backward
        prev = curr       # 3. Move prev up
        curr = nxt        # 4. Move curr up
    return prev
```

#### C++
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* next = curr->next; // 1. Save what's ahead
        curr->next = prev;           // 2. Flip connection backward
        prev = curr;                 // 3. Move prev up
        curr = next;                 // 4. Move curr up
    }
    return prev;
}
```
</details>

---

### Solution 6: Linked List Cycle (Floyd's Cycle Finding Algorithm)

**Intuition:**
If there is a cycle, a fast pointer moving 2 steps at a time will eventually meet a slow pointer moving 1 step at a time inside the loop (like a fast runner lapping a slow runner on a track). If there is no cycle, `fast` will hit `null`.

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true; // Met -> Cycle exists
        }
    }
    return false; // Reached end -> No cycle
}
```

#### Python
```python
def hasCycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

#### C++
```cpp
bool hasCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```
</details>

---

## 🟡 Day 2: Interview Combinations

These problems combine Day 1 skills to solve complex scenarios.

### Solution 7: Remove Nth Node From End of List

**Intuition:**
We need to remove the $N$-th node from the end. 
- Use a `dummy` node pointing to the head.
- Place `slow` and `fast` pointers at `dummy`.
- Move `fast` forward by $N + 1$ steps. This establishes a gap of size $N$ between `slow` and `fast`.
- Move both together at the same speed. When `fast` reaches `null`, `slow` will point to the node **just before** the target.
- Perform insertion deletion: `slow.next = slow.next.next`.

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = dummy;
    
    // Step 1: Move fast pointer N+1 steps forward
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    // Step 2: Move both together until fast reaches null
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    // Step 3: Delete node
    slow.next = slow.next.next;
    return dummy.next;
}
```

#### Python
```python
def removeNthFromEnd(head, n):
    dummy = ListNode(0)
    dummy.next = head
    slow = fast = dummy
    
    for _ in range(n + 1):
        fast = fast.next
        
    while fast:
        slow = slow.next
        fast = fast.next
        
    slow.next = slow.next.next
    return dummy.next
```

#### C++
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode* dummy = new ListNode(0);
    dummy->next = head;
    ListNode* slow = dummy;
    ListNode* fast = dummy;
    
    for (int i = 0; i <= n; i++) {
        fast = fast->next;
    }
    
    while (fast) {
        slow = slow->next;
        fast = fast->next;
    }
    
    slow->next = slow->next->next;
    ListNode* result = dummy->next;
    delete dummy; // Clean heap allocation
    return result;
}
```
</details>

---

### Solution 8: Merge Two Sorted Lists

**Complexity:**
- **Time:** $O(n + m)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            tail.next = list1;
            list1 = list1.next;
        } else {
            tail.next = list2;
            list2 = list2.next;
        }
        tail = tail.next;
    }
    
    // Attach remainder nodes
    tail.next = (list1 != null) ? list1 : list2;
    return dummy.next;
}
```

#### Python
```python
def mergeTwoLists(list1, list2):
    dummy = ListNode(0)
    tail = dummy
    
    while list1 and list2:
        if list1.val <= list2.val:
            tail.next = list1
            list1 = list1.next
        else:
            tail.next = list2
            list2 = list2.next
        tail = tail.next
        
    tail.next = list1 if list1 else list2
    return dummy.next
```

#### C++
```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    ListNode dummy(0);
    ListNode* tail = &dummy;
    
    while (list1 && list2) {
        if (list1->val <= list2->val) {
            tail->next = list1;
            list1 = list1->next;
        } else {
            tail->next = list2;
            list2 = list2->next;
        }
        tail = tail->next;
    }
    
    tail->next = list1 ? list1 : list2;
    return dummy.next;
}
```
</details>

---

### Solution 9: Add Two Numbers

**Complexity:**
- **Time:** $O(\max(n, m))$
- **Space:** $O(\max(n, m))$ to construct the result list.

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    int carry = 0;
    
    while (l1 != null || l2 != null || carry > 0) {
        int sum = carry;
        if (l1 != null) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2 != null) {
            sum += l2.val;
            l2 = l2.next;
        }
        
        tail.next = new ListNode(sum % 10);
        carry = sum / 10;
        tail = tail.next;
    }
    return dummy.next;
}
```

#### Python
```python
def addTwoNumbers(l1, l2):
    dummy = ListNode(0)
    tail = dummy
    carry = 0
    
    while l1 or l2 or carry:
        val = carry
        if l1:
            val += l1.val
            l1 = l1.next
        if l2:
            val += l2.val
            l2 = l2.next
            
        tail.next = ListNode(val % 10)
        carry = val // 10
        tail = tail.next
        
    return dummy.next
```

#### C++
```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* dummy = new ListNode(0);
    ListNode* tail = dummy;
    int carry = 0;
    
    while (l1 || l2 || carry) {
        int sum = carry;
        if (l1) {
            sum += l1->val;
            l1 = l1->next;
        }
        if (l2) {
            sum += l2->val;
            l2 = l2->next;
        }
        tail->next = new ListNode(sum % 10);
        carry = sum / 10;
        tail = tail->next;
    }
    return dummy->next;
}
```
</details>

---

### Solution 10: Linked List Cycle II

**Intuition:**
1. Run Floyd's algorithm to detect if a cycle exists. Let the meeting point of `slow` and `fast` be node $M$.
2. Reset `slow` back to `head`. Keep `fast` at meeting point $M$.
3. Move both pointers forward 1 step at a time. The node where they meet is the start of the cycle.
*(Proof: Let distance from head to cycle start be $D$. Let cycle length be $C$. The distance from cycle start to meeting point is $K$. The math shows that $D = x \cdot C - K$, which means the distance from head to cycle start is congruent to the distance from the meeting point to cycle start).*

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode detectCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            // Cycle detected. Find start node.
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow; // Start of cycle
        }
    }
    return null; // No cycle
}
```

#### Python
```python
def detectCycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            slow = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow
    return None
```

#### C++
```cpp
ListNode* detectCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow->next;
                fast = fast->next;
            }
            return slow;
        }
    }
    return nullptr;
}
```
</details>

---

### Solution 11: Palindrome Linked List

**Intuition:**
1. Find the middle of the list using slow/fast pointers.
2. Reverse the second half of the list starting from the middle node.
3. Compare the values of the first half (from `head`) and the reversed second half.

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public boolean isPalindrome(ListNode head) {
    // 1. Find middle
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // 2. Reverse second half
    ListNode prev = null;
    ListNode curr = slow;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    
    // 3. Compare halves
    ListNode left = head;
    ListNode right = prev;
    while (right != null) {
        if (left.val != right.val) {
            return false;
        }
        left = left.next;
        right = right.next;
    }
    return true;
}
```

#### Python
```python
def isPalindrome(head):
    # 1. Find middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
    # 2. Reverse second half
    prev = None
    curr = slow
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt
        
    # 3. Compare halves
    left = head
    right = prev
    while right:
        if left.val != right.val:
            return False
        left = left.next
        right = right.next
    return True
```

#### C++
```cpp
bool isPalindrome(ListNode* head) {
    // 1. Find middle
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    
    // 2. Reverse second half
    ListNode* prev = nullptr;
    ListNode* curr = slow;
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    
    // 3. Compare halves
    ListNode* left = head;
    ListNode* right = prev;
    while (right) {
        if (left->val != right->val) return false;
        left = left->next;
        right = right->next;
    }
    return true;
}
```
</details>

---

### Solution 12: Odd Even Linked List

**Intuition:**
We create two separate chains: one for nodes at odd indices, one for nodes at even indices.
- Initialize `odd = head` and `even = head.next`.
- Store `evenHead = even` to connect the end of the odd chain to the beginning of the even chain later.
- Loop and decouple nodes:
  `odd.next = even.next; odd = odd.next;`
  `even.next = odd.next; even = even.next;`
- Conclude by linking `odd.next = evenHead`.

**Complexity:**
- **Time:** $O(n)$
- **Space:** $O(1)$

<details>
<summary>💻 Multi-Language Code</summary>

#### Java
```java
public ListNode oddEvenList(ListNode head) {
    if (head == null) return null;
    ListNode odd = head;
    ListNode even = head.next;
    ListNode evenHead = even;
    
    while (even != null && even.next != null) {
        odd.next = even.next;
        odd = odd.next;
        even.next = odd.next;
        even = even.next;
    }
    odd.next = evenHead;
    return head;
}
```

#### Python
```python
def oddEvenList(head):
    if not head:
        return None
    odd = head
    even = head.next
    even_head = even
    
    while even and even.next:
        odd.next = even.next
        odd = odd.next
        even.next = odd.next
        even = even.next
        
    odd.next = even_head
    return head
```

#### C++
```cpp
ListNode* oddEvenList(ListNode* head) {
    if (!head) return nullptr;
    ListNode* odd = head;
    ListNode* even = head->next;
    ListNode* evenHead = even;
    
    while (even && even->next) {
        odd->next = even->next;
        odd = odd->next;
        even->next = odd->next;
        even = even->next;
    }
    odd->next = evenHead;
    return head;
}
```
</details>

<details>
<summary>📋 Step-by-Step Dry Run</summary>

Input: `head = [1, 2, 3, 4, 5]`

- Initial pointers: `odd` $\to 1$, `even` $\to 2$, `evenHead` $\to 2$
- **Iteration 1 (`even = 2`):**
  - `odd.next = even.next` (3) $\implies$ `odd` chain: $1 \to 3$
  - `odd = 3`
  - `even.next = odd.next` (4) $\implies$ `even` chain: $2 \to 4$
  - `even = 4`
- **Iteration 2 (`even = 4`):**
  - `odd.next = even.next` (5) $\implies$ `odd` chain: $1 \to 3 \to 5$
  - `odd = 5`
  - `even.next = odd.next` (`null`) $\implies$ `even` chain: $2 \to 4 \to \text{null}$
  - `even = null`
- Loop terminates (`even == null`).
- Connect chains: `odd.next = evenHead` ($5 \to 2$).

**Result:** `[1, 3, 5, 2, 4]` ✅
</details>

---

## 🎓 Viva Questions & Answers

### Q1: What is the primary difference in memory allocation between Arrays and Linked Lists?
**Answer:**
- **Array:** Continuous block of memory allocated contiguously. Size is fixed (or requires re-allocation), and random access is $O(1)$.
- **Linked List:** Dynamic nodes scattered non-contiguously in Heap memory connected by reference pointers. Insertion/deletion is $O(1)$ if pointer reference is known, but element access is $O(n)$.

### Q2: What is a Dummy Head Pointer Node, and why is it useful?
**Answer:**
A **Dummy Node** is a placeholder node (`ListNode dummy = new ListNode(0)`) placed before `head`. It simplifies code by eliminating edge cases when inserting or deleting at the very beginning of the list, allowing uniform pointer operations. `dummy.next` returns the modified actual head.

### Q3: Why is Floyd's Cycle Detection algorithm guaranteed to find a cycle if one exists?
**Answer:**
Let the fast pointer move 2 steps and slow pointer move 1 step. In each step, the distance between `fast` and `slow` inside the cycle decreases by $1$ node ($(2 - 1) = 1$). If the cycle length is $C$, `fast` will catch up to `slow` in at most $C$ steps after both enter the cycle.

### Q4: Compare Singly Linked List vs Doubly Linked List vs Circular Linked List.
**Answer:**
- **Singly Linked List:** Each node has a `val` and `next` pointer. Uses less memory per node, but can only traverse forward.
- **Doubly Linked List:** Each node has `val`, `next`, and `prev` pointers. Allows bi-directional traversal and $O(1)$ deletion given node reference, but uses extra pointer memory.
- **Circular Linked List:** Tail node's `next` points back to `head` (or head's `prev` points to tail). Useful for round-robin scheduling algorithms.

### Q5: How do you reverse a Singly Linked List in $O(n)$ time and $O(1)$ space?
**Answer:**
Maintain three pointers: `prev = null`, `curr = head`, and `next = null`.
In a loop while `curr != null`:
1. Save `next = curr.next`.
2. Reverse link: `curr.next = prev`.
3. Advance: `prev = curr`, `curr = next`.
Return `prev` as the new head.

---

## ⚠️ Beginner Pitfalls & Common Mistakes

1. **Null Pointer Exceptions (Dereferencing Null):**
   - The #1 source of crashes in linked list code.
   - Doing `curr = curr.next` when `curr` is `null` will throw a crash.
   - Doing `fast.next.next` when `fast` is `null` or `fast.next` is `null` will throw a crash. Always write bounds guard checks first: `while (fast != null && fast.next != null)`.

2. **Pointer Loss (The Orphan Trap):**
   - If you do `head.next = prev` without saving the original `head.next` pointer first, you sever the link to the rest of the list. That remaining list gets orphaned in memory.
   - **Rule:** Always record pointers to variables *before* you overwrite them.

---

> 👉 Next, open `11-Stack.md` to explore LIFO stack operations! 💪

