### 1.题目描述

 **[剑指 Offer II 053. 二叉搜索树中的中序后继](https://leetcode-cn.com/problems/P5rCT8/)** 
 
给定一棵二叉搜索树和其中的一个节点 p ，找到该节点在树中的`中序后继`。如果节点没有中序后继，请返回 null 。

节点 p 的后继是值比 p.val 大的节点中键值最小的节点，即按中序遍历的顺序节点 p 的下一个节点。


 **示例 1：**
 
 ![图片](https://user-images.githubusercontent.com/42907149/144364744-fad4d029-3044-4dc2-9ea2-a14cd1f8fb10.png)
```

输入：root = [2,1,3], p = 1
输出：2
解释：这里 1 的中序后继是 2。请注意 p 和返回值都应是 TreeNode 类型。
```

**示例 2：**

![图片](https://user-images.githubusercontent.com/42907149/144364792-c7a0bc04-7f85-4aba-a3e4-a41170ad8306.png)
```
输入：root = [5,3,6,2,4,null,null,1], p = 6
输出：null
解释：因为给出的节点没有中序后继，所以答案就返回 null 了。
```

  

**注意**：本题与lc 285 题相同： https://leetcode-cn.com/problems/inorder-successor-in-bst/




-------------

### 2.解题思路：


`节点 p 的后继是值比 p->val 大的节点中键值最小的节点，即按中序遍历的顺序节点 p 的下一个节点。`

可以每到达一个节点就比较根节点和节点p的值

* 如果当前节点值小于或等于节点p->val，则节点p的下一个节点就应该在它的右子树。

* 如果当前节点值比节点p值大，但节点p的下一个节点是所有比它大的节点值中最小的，就找它的左子树。



### 3.代码实现：

**JAVA版本：**
```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode cur = root;
        TreeNode res = null;
        while(cur!= null){
            if(cur.val > p.val){
                res = cur;
                cur = cur.left;

            }else{
                cur = cur.right;

            }

        }
        return res;
        
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        TreeNode* cur = root;
        TreeNode* res = NULL;
        while(cur!= NULL){
            if(cur->val > p->val){
                res = cur;
                cur = cur->left;
            }else{
                cur = cur->right;
            }

        }
        return res;
    }
};
```

