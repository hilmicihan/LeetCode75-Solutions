# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
🧠 When deleting a node from a Binary Search Tree (BST), the main challenge is maintaining the BST properties. The steps involve:

- Searching for the node to be deleted.
- Handling different scenarios based on whether the node has no children, one child, or two children.
- If the node has two children, finding the inorder successor (the smallest node in the right subtree) to replace the node's value.

# Approach
<!-- Describe your approach to solving the problem. -->
🔍 **Search for the Node to Delete**:
- Traverse the BST to locate the node to be deleted.
- Use recursion to navigate through the left or right subtree based on the node's value.

🔨 **Delete the Node**:
- **Case 1: Node with No Children**:
  - Simply remove the node by returning `null`.
- **Case 2: Node with One Child**:
  - Replace the node with its non-null child.
- **Case 3: Node with Two Children**:
  - Find the inorder successor.
  - Replace the node's value with the inorder successor's value.
  - Recursively delete the inorder successor.

🧩 **Helper Function**:
- `minValueNode`: Finds the smallest node in the right subtree, used to locate the inorder successor.

# Complexity
- Time complexity: 
  - 💡 $$O(h)$$, where $$h$$ is the height of the tree. In the worst case (unbalanced tree), it can be $$O(n)$$, and in the best case (balanced tree), it is $$O(\log n)$$.
  
- Space complexity:
  - 💾 $$O(h)$$ due to the recursion stack. In the worst case, it can be $$O(n)$$, and in the best case, $$O(\log n)$$.

# Code
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return root;
        
        if (key < root->val) {
            root->left = deleteNode(root->left, key);
        } else if (key > root->val) {
            root->right = deleteNode(root->right, key);
        } else {
            if (!root->left) {
                TreeNode* temp = root->right;
                delete root;
                return temp;
            } else if (!root->right) {
                TreeNode* temp = root->left;
                delete root;
                return temp;
            } else {
                TreeNode* temp = minValueNode(root->right);
                root->val = temp->val;
                root->right = deleteNode(root->right, temp->val);
            }
        }
        return root;
    }

private:
    TreeNode* minValueNode(TreeNode* node) {
        TreeNode* current = node;
        while (current && current->left) {
            current = current->left;
        }
        return current;
    }
};
```

# Walkthrough Example
Let's walk through an example step-by-step:

### Example 1

#### Input:
- `root = [5, 3, 6, 2, 4, null, 7]`
- `key = 3`

#### Initial Tree:
```
        5
       / \
      3   6
     / \   \
    2   4   7
```

#### Steps:
1. **Search for Node**:
   - Start at root (5). `3 < 5`, move left.
   - At node 3. `3 == 3`, node found.

2. **Delete Node**:
   - Node 3 has two children (2 and 4).
   - Find inorder successor (smallest in right subtree of 3), which is 4.
   - Replace node 3's value with 4.
   - Recursively delete node 4.

3. **Recursive Deletion**:
   - Node 4 has no children, simply remove it.

#### Updated Tree:
```
        5
       / \
      4   6
     /     \
    2       7
```

#### Output:
- `[5, 4, 6, 2, null, null, 7]`

### Example 2

#### Input:
- `root = [5, 3, 6, 2, 4, null, 7]`
- `key = 0`

#### Initial Tree:
```
        5
       / \
      3   6
     / \   \
    2   4   7
```

#### Steps:
1. **Search for Node**:
   - Start at root (5). `0 < 5`, move left.
   - At node 3. `0 < 3`, move left.
   - At node 2. `0 < 2`, move left.
   - Reached null node, key 0 not found.

#### Tree Remains Unchanged:
```
        5
       / \
      3   6
     / \   \
    2   4   7
```

#### Output:
- `[5, 3, 6, 2, 4, null, 7]`

### Example 3

#### Input:
- `root = []`
- `key = 0`

#### Initial Tree:
- Empty

#### Steps:
- Nothing to delete as the tree is empty.

#### Output:
- `[]`
