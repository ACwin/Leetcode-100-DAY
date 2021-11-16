### 1.题目描述

 **[剑指 Offer II 047. 二叉树剪枝](https://leetcode-cn.com/problems/pOCWxh/)** 
 
 ![图片](https://user-images.githubusercontent.com/42907149/141930105-25145b5d-b659-4257-9a11-ea1874103f7d.png)
 
**提示:**

- 二叉树的节点个数的范围是 [1,200]
- 二叉树节点的值只会是 0 或 1

 

**注意：** 本题与lc 814 题相同：https://leetcode-cn.com/problems/binary-tree-pruning/

-------------

### 2.解题思路：

使用**后序遍历方法**，整个递归函数的过程都是先处理左右孩子节点再处理当前节点，这是后序遍历的一种变型。
我们从叶子节点网上逆推：
1. 左子树为空
2. 右子树为空
3. node.val == 0
当满足以上三个条件时，将该叶子节点删除(node = null)，即可。


### 3.代码实现：

**JAVA版本：**
```Java
/**
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode pruneTree(TreeNode root) {
        if( root == null) {
            return root;
        }
        root.left = pruneTree(root.left);
        root.right = pruneTree(root.right);
        if (root.val == 0 && root.left == null && root.right == null){
            root = null;
        }
        return root;
    }
}
```

**C++版本：**
```C++
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
    TreeNode* pruneTree(TreeNode* root) {
        if (root == nullptr) {
            return nullptr;
        }
        TreeNode* left = pruneTree(root->left);
        TreeNode* right = pruneTree(root->right);
        if (root->val == 0 && left == nullptr && right == nullptr) {
            return nullptr;
        }
        root->left = left;
        root->right = right;
        return root;
    }
};
```

**Python版本：**

```Python
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pruneTree(self, root):
        if not root:
            return root
        root.left = self.pruneTree(root.left)
        root.right = self.pruneTree(root.right)
        if root.val == 0 and not root.left and not root.right:
            return None
        return root
```
