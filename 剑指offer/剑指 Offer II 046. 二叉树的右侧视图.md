### 1.题目描述

 **[剑指 Offer II 046. 二叉树的右侧视图](https://leetcode-cn.com/problems/WNC0Lk/)** 
 
给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。
 

**示例 1:**
 
![图片](https://user-images.githubusercontent.com/42907149/143998934-73af4fbb-ae03-47f3-b8b9-14957370469c.png)

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
```

**提示:**

- 二叉树的节点个数的范围是 [0,100]
- -100 <= Node.val <= 100 

 

注意：本题与lc 199 题相同：https://leetcode-cn.com/problems/binary-tree-right-side-view/


-------------

### 2.解题思路：

> **BFS逐层遍历**



### 3.代码实现：

**JAVA版本：**
```Java
/**
 * Definition for a binary tree node.
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
    public List<Integer> rightSideView(TreeNode root) {
       
        Queue<TreeNode> queue1=new LinkedList<>();//存储当前层的节点
        Queue<TreeNode> queue2=new LinkedList<>();//存储下一层的节点
        List<Integer> list=new LinkedList<>();
        if(root==null) return list;
        queue1.offer(root);
        while(!queue1.isEmpty()){//先左边再右边，node指向当前层最右节点
        TreeNode node=queue1.poll();

             if(node.left!=null){
                 queue2.offer(node.left);
             }
             if(node.right!=null){
                 queue2.offer(node.right);
             }
             if(queue1.isEmpty()){//若当前层遍历完毕，则换乘下一行
             queue1=queue2;//换下一行
             queue2=new LinkedList<>();//重置
             list.add(node.val);//将当前层的最后一个节点压入，即最右边节点,node节点
             }

        }
        return list;

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
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> que;
        vector<int> res;
        if(root ==nullptr) return res;
        que.push(root);
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                 if(i == size-1)
                    res.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);

             }
        }
        return res;

    }
};
```

**BFS题型：**

* 102.二叉树的层序遍历
* 107.二叉树的层次遍历II
* 199.二叉树的右视图
* 637.二叉树的层平均值
* 429.N叉树的前序遍历
* 515.在每个树行中找最大值
* 116.填充每个节点的下一个右侧节点指针
* 117.填充每个节点的下一个右侧节点指针II
* 104.二叉树的最大深度
* 111.二叉树的最小深度
* 剑指 Offer II 046. 二叉树的右侧视图
* 剑指 Offer II 045. 二叉树最底层最左边的值
* 剑指 Offer II 044. 二叉树每层的最大值
