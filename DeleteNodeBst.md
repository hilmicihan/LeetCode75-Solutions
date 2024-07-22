<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BST Node Deletion</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        pre {
            background: #f4f4f4;
            padding: 1em;
            border: 1px solid #ddd;
            overflow: auto;
        }
    </style>
</head>
<body>
    <h1>Intuition</h1>
    <!-- Describe your first thoughts on how to solve this problem. -->
    <p>🧠 When deleting a node from a Binary Search Tree (BST), the main challenge is maintaining the BST properties. The steps involve:</p>
    <ul>
        <li>Searching for the node to be deleted.</li>
        <li>Handling different scenarios based on whether the node has no children, one child, or two children.</li>
        <li>If the node has two children, finding the inorder successor (the smallest node in the right subtree) to replace the node's value.</li>
    </ul>

    <h1>Approach</h1>
    <!-- Describe your approach to solving the problem. -->
    <p>🔍 <strong>Search for the Node to Delete</strong>:</p>
    <ul>
        <li>Traverse the BST to locate the node to be deleted.</li>
        <li>Use recursion to navigate through the left or right subtree based on the node's value.</li>
    </ul>

    <p>🔨 <strong>Delete the Node</strong>:</p>
    <ul>
        <li><strong>Case 1: Node with No Children</strong>:<br>
            Simply remove the node by returning <code>null</code>.
        </li>
        <li><strong>Case 2: Node with One Child</strong>:<br>
            Replace the node with its non-null child.
        </li>
        <li><strong>Case 3: Node with Two Children</strong>:<br>
            Find the inorder successor.<br>
            Replace the node's value with the inorder successor's value.<br>
            Recursively delete the inorder successor.
        </li>
    </ul>

    <p>🧩 <strong>Helper Function</strong>:</p>
    <ul>
        <li><code>minValueNode</code>: Finds the smallest node in the right subtree, used to locate the inorder successor.</li>
    </ul>

    <h1>Complexity</h1>
    <p>💡 <strong>Time complexity</strong>:<br>
    <code>O(h)</code>, where <code>h</code> is the height of the tree. In the worst case (unbalanced tree), it can be <code>O(n)</code>, and in the best case (balanced tree), it is <code>O(log n)</code>.</p>
    
    <p>💾 <strong>Space complexity</strong>:<br>
    <code>O(h)</code> due to the recursion stack. In the worst case, it can be <code>O(n)</code>, and in the best case, <code>O(log n)</code>.</p>

    <h1>Code</h1>
    <pre>
<code>
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
</code>
    </pre>

    <h1>Walkthrough Example</h1>
    <p>Let's walk through an example step-by-step:</p>

    <h2>Example 1</h2>

    <h3>Input:</h3>
    <ul>
        <li><code>root = [5, 3, 6, 2, 4, null, 7]</code></li>
        <li><code>key = 3</code></li>
    </ul>

    <h3>Initial Tree:</h3>
    <pre>
        5
       / \
      3   6
     / \   \
    2   4   7
    </pre>

    <h3>Steps:</h3>
    <ol>
        <li><strong>Search for Node</strong>:
            <ul>
                <li>Start at root (5). <code>3 < 5</code>, move left.</li>
                <li>At node 3. <code>3 == 3</code>, node found.</li>
            </ul>
        </li>
        <li><strong>Delete Node</strong>:
            <ul>
                <li>Node 3 has two children (2 and 4).</li>
                <li>Find inorder successor (smallest in right subtree of 3), which is 4.</li>
                <li>Replace node 3's value with 4.</li>
                <li>Recursively delete node 4.</li>
            </ul>
        </li>
        <li><strong>Recursive Deletion</strong>:
            <ul>
                <li>Node 4 has no children, simply remove it.</li>
            </ul>
        </li>
    </ol>

    <h3>Updated Tree:</h3>
    <pre>
        5
       / \
      4   6
     /     \
    2       7
    </pre>

    <h3>Output:</h3>
    <ul>
        <li><code>[5, 4, 6, 2, null, null, 7]</code></li>
    </ul>

    <h2>Example 2</h2>

    <h3>Input:</h3>
    <ul>
        <li><code>root = [5, 3, 6, 2, 4, null, 7]</code></li>
        <li><code>key = 0</code></li>
    </ul>

    <h3>Initial Tree:</h3>
    <pre>
        5
       / \
      3   6
     / \   \
    2   4   7
    </pre>

    <h3>Steps:</h3>
    <ol>
        <li><strong>Search for Node</strong>:
            <ul>
                <li>Start at root (5). <code>0 < 5</code>, move left.</li>
                <li>At node 3. <code>0 < 3</code>, move left.</li>
                <li>At node 2. <code>0 < 2</code>, move left.</li>
                <li>Reached null node, key 0 not found.</li>
            </ul>
        </li>
    </ol>

    <h3>Tree Remains Unchanged:</h3>
    <pre>
        5
       / \
      3   6
     / \   \
    2   4   7
    </pre>

    <h3>Output:</h3>
    <ul>
        <li><code>[5, 3, 6, 2, 4, null, 7]</code></li>
    </ul>

    <h2>Example 3</h2>

    <h3>Input:</h3>
    <ul>
        <li><code>root = []</code></li>
        <li><code>key = 0</code></li>
    </ul>

    <h3>Initial Tree:</h3>
    <p>Empty</p>

    <h3>Steps:</h3>
    <p>Nothing to delete as the tree is empty.</p>

    <h3>Output:</h3>
    <ul>
        <li><code>[]</code></li>
    </ul>
</body>
</html>
