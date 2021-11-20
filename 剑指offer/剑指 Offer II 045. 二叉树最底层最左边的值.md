### 1.题目描述

 **[剑指 Offer II 045. 二叉树最底层最左边的值](https://leetcode-cn.com/problems/LwUNpT/)** 
 

 ![图片](https://user-images.githubusercontent.com/42907149/142722256-27cdb4a0-2531-4421-aa92-38394de2aec3.png)


**提示:**

- 二叉树的节点个数的范围是 [1,104]
- -231 <= Node.val <= 231 - 1 

**注意**：本题与lc 513 题相同： https://leetcode-cn.com/problems/find-bottom-left-tree-value/

-------------

### 2.解题思路：

>BFS逐层遍历,每次都记录当前层的最左端值，那么等遍历完二叉树，记录的当前层的最左端节点值就是最底层的最左端节点值

### 3.代码实现：

**JAVA版本：**
```Java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue1 = new LinkedList<>();
        Queue<TreeNode> queue2 = new LinkedList<>();
        queue1.offer(root);
        int bottomLeft = root.val;
        while (!queue1.isEmpty()) {
            TreeNode node = queue1.poll();
            if(node.left != null){
                queue2.offer(node.left);
            }
            if(node.right != null){
                queue2.offer(node.right);
            }
            if(queue1.isEmpty()){
                queue1 = queue2;
                queue2 = new LinkedList<>();
                if(!queue1.isEmpty()){
                    bottomLeft = queue1.peek().val;
                }
            }
        }
         return bottomLeft;
       
    }
}
```
**C++版本：**
```C++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        int bottomLeft = 0;
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; ++i) {
                TreeNode* node = que.front();
                que.pop();
                if (i == 0) {
                    bottomLeft = node->val;
                }
                if (node->left != nullptr) {
                    que.push(node->left);
                }
                if (node->right != nullptr) {
                    que.push(node->right);
                }
            }
        }    
        return bottomLeft;   
    }
};
```
