### 1.题目描述

 **[剑指 Offer II 052. 展平二叉搜索树](https://leetcode-cn.com/problems/NYBBNL/)** 
 
给你一棵二叉搜索树，请 按中序遍历 将其重新排列为一棵递增顺序搜索树，使树中最左边的节点成为树的根节点，并且每个节点没有左子节点，只有一个右子节点。

**示例 1：**

 
![图片](https://user-images.githubusercontent.com/42907149/144382996-3225ff39-cc38-4f89-a670-f56c08c1f318.png)


 **提示：**

* 树中节点数的取值范围是 [1, 100]
* 0 <= Node.val <= 1000

**注意**：本题与lc 897 题相同： https://leetcode-cn.com/problems/increasing-order-search-tree/

-------------

### 2.解题思路：
> 用二叉树的中序遍历（左中右）每遍历到一个节点就要把前一个节点的右子节点指向它

### 3.代码实现：

**JAVA版本：**
```Java
class Solution {
    TreeNode head = null;
    TreeNode pre = null; //前一节点
    public TreeNode increasingBST(TreeNode root) {
        
        
        if(root == null){
            return null;
        }
        increasingBST(root.left);
        if(pre != null){
            pre.right = root;
            root.left = null;

        }
        else{
            head = root;
        }
        pre = root;
        increasingBST(root.right);
        return head;

    }
};
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
    TreeNode* head = nullptr;
    TreeNode* pre = nullptr; //前一节点
    TreeNode* increasingBST(TreeNode* root) {
        if(root == nullptr){
            return nullptr;
        }
        increasingBST(root->left);
        if(pre != nullptr){
            pre->right = root;
            root->left = nullptr;

        }
        else{
            head = root;
        }
        pre = root;
        increasingBST(root->right);
        return head;

    }
};
```

