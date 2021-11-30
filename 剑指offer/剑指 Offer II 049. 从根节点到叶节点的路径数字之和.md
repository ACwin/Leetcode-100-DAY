### 1.题目描述

 **[剑指 Offer II 049. 从根节点到叶节点的路径数字之和](https://leetcode-cn.com/problems/3Etpl5/)** 
 
给定一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

- 例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。

计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

**示例 1：**

![图片](https://user-images.githubusercontent.com/42907149/144080562-457a6d1d-501c-474d-8b31-a005c1d47f0c.png)

```
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
```

**示例 2：**

![图片](https://user-images.githubusercontent.com/42907149/144080793-09411ed3-fb72-4cd4-9ead-285377d0e619.png)
```
输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
```
***提示：**

* 树中节点的数目在范围 [1, 1000] 内
* 0 <= Node.val <= 9
* 树的深度不超过 10

**注意**：本题与lc 129 题相同： https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/

-------------

### 2.解题思路：

**方法一：深度优先搜索**

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
    public int sumNumbers(TreeNode root) {
        return dfs(root,0);

    }
    private int dfs(TreeNode root,int path){
        if(root == null){
            return 0;
        }
        path = path * 10 + root.val;
        if(root.left == null && root.right == null){
            return path;
        }
        return dfs(root.left,path) + dfs(root.right,path);
    }
    

}
```
**C++版本：**
```C++
class Solution {
public:
    int dfs(TreeNode* root, int prevSum) {
        if (root == nullptr) {
            return 0;
        }
        int sum = prevSum * 10 + root->val;
        if (root->left == nullptr && root->right == nullptr) {
            return sum;
        } else {
            return dfs(root->left, sum) + dfs(root->right, sum);
        }
    }
    int sumNumbers(TreeNode* root) {
        return dfs(root, 0);
    }
};

```

**Python版本：**

```Python
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        def dfs(root: TreeNode, prevTotal: int) -> int:
            if not root:
                return 0
            total = prevTotal * 10 + root.val
            if not root.left and not root.right:
                return total
            else:
                return dfs(root.left, total) + dfs(root.right, total)

        return dfs(root, 0)

```

**复杂度分析：**

- 时间复杂度：O(n)，其中 n 是二叉树的节点个数。对每个节点访问一次。
- 空间复杂度：O(n)，其中 n是二叉树的节点个数。空间复杂度主要取决于递归调用的栈空间，递归栈的深度等于二叉树的高度，最坏情况下，二叉树的高度等于节点个数，空间复杂度为 O(n)。

**方法二：广度优先搜索**

**JAVA版本：**
```Java
class Solution {
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int sum = 0;
        Queue<TreeNode> nodeQueue = new LinkedList<TreeNode>();
        Queue<Integer> numQueue = new LinkedList<Integer>();
        nodeQueue.offer(root);
        numQueue.offer(root.val);
        while (!nodeQueue.isEmpty()) {
            TreeNode node = nodeQueue.poll();
            int num = numQueue.poll();
            TreeNode left = node.left, right = node.right;
            if (left == null && right == null) {
                sum += num;
            } else {
                if (left != null) {
                    nodeQueue.offer(left);
                    numQueue.offer(num * 10 + left.val);
                }
                if (right != null) {
                    nodeQueue.offer(right);
                    numQueue.offer(num * 10 + right.val);
                }
            }
        }
        return sum;
    }
}


```
**C++版本：**
```C++
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int sum = 0;
        queue<TreeNode*> nodeQueue;
        queue<int> numQueue;
        nodeQueue.push(root);
        numQueue.push(root->val);
        while (!nodeQueue.empty()) {
            TreeNode* node = nodeQueue.front();
            int num = numQueue.front();
            nodeQueue.pop();
            numQueue.pop();
            TreeNode* left = node->left;
            TreeNode* right = node->right;
            if (left == nullptr && right == nullptr) {
                sum += num;
            } else {
                if (left != nullptr) {
                    nodeQueue.push(left);
                    numQueue.push(num * 10 + left->val);
                }
                if (right != nullptr) {
                    nodeQueue.push(right);
                    numQueue.push(num * 10 + right->val);
                }
            }
        }
        return sum;
    }
};

```

**Python版本：**

```Python
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        if not root:
            return 0

        total = 0
        nodeQueue = collections.deque([root])
        numQueue = collections.deque([root.val])
        
        while nodeQueue:
            node = nodeQueue.popleft()
            num = numQueue.popleft()
            left, right = node.left, node.right
            if not left and not right:
                total += num
            else:
                if left:
                    nodeQueue.append(left)
                    numQueue.append(num * 10 + left.val)
                if right:
                    nodeQueue.append(right)
                    numQueue.append(num * 10 + right.val)

        return total


```
**复杂度分析**

- 时间复杂度：O(n)，其中 n 是二叉树的节点个数。对每个节点访问一次。

- 空间复杂度：O(n)，其中 n 是二叉树的节点个数。空间复杂度主要取决于队列，每个队列中的元素个数不会超过 n。
