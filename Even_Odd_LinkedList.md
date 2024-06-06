# Intuition
**🧠 First Thoughts:** To rearrange the linked list into odd and even indexed nodes, use two pointers to separately track odd and even nodes. Connect odd nodes followed by even nodes.

# Approach
**🚀 Approach:**
1. **Initialize Pointers:** Use three pointers: `odd`, `even`, and `even_head`.
2. **Separate Lists:** Traverse the list, linking odd nodes together and even nodes together.
3. **Merge Lists:** Connect the end of the odd list to the head of the even list.

# Complexity
- **⏱️ Time complexity:** 
  $$O(n)$$ - Only one pass through the list is needed.

- **📦 Space complexity:** 
  $$O(1)$$ - Only a constant amount of extra space is used.

# Code
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        // If the list is empty or has only one node, return head.
        if (!head || !head->next)
            return head;

        // Initialize pointers for odd and even nodes.
        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* even_head = even; // Save the head of the even list.

        // Traverse the list to separate odd and even nodes.
        while (even && even->next) {
            odd->next = even->next; // Link the next odd node.
            odd = odd->next;        // Move odd pointer to the next odd node.
            even->next = odd->next; // Link the next even node.
            even = even->next;      // Move even pointer to the next even node.
        }

        // Connect the end of the odd list to the head of the even list.
        odd->next = even_head;

        // Return the head of the modified linked list.
        return head;
    }
};
```

# Walkthrough Example
**Example Input:** [1, 2, 3, 4, 5]

1. **Initialization:** 
   - `odd` = 1 (index 0)
   - `even` = 2 (index 1)
   - `even_head` = 2 (index 1)
2. **First Iteration:**
   - `odd->next` points to 3 (index 2)
   - `odd` moves to 3 (index 2)
   - `even->next` points to 4 (index 3)
   - `even` moves to 4 (index 3)
3. **Second Iteration:**
   - `odd->next` points to 5 (index 4)
   - `odd` moves to 5 (index 4)
   - `even->next` points to `nullptr` (end of list)
   - `even` moves to `nullptr`
4. **Merge Lists:** `odd->next` points to `even_head` (2)

**Output:** [1, 3, 5, 2, 4]

****
