### 1.题目描述

 **[剑指 Offer II 051. 节点之和最大的路径](https://leetcode-cn.com/problems/jC7MId/)** 
 
路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给定一个二叉树的根节点 root ，返回其 最大路径和，即所有路径上节点值之和的最大值。

**示例 1：**

 ![图片](https://user-images.githubusercontent.com/42907149/144364036-7b2944e8-52df-46db-bf61-ffd66d115402.png)
 
```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

**提示：**

- 树中节点数目范围是 [1, 3 * 104]
- -1000 <= Node.val <= 1000

**注意**：本题与lc 124 题相同： https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/


-------------

### 2.解题思路：

递归:
> 因为若 left 和 right 为负数那么就应该舍弃，为了方便取 right = max(0, right) ，left = max(0, left)。则节点 a 返回的最大 “直径” 和的值就是 a + max(left, right)。因为题目中要求所有符合条件（所有的 “直径” 和 “曲径”）的路径的最大路径和，所有记录所有 “曲径” 的最大值就是结果值，ret = max(ret, left + right + root->val)。

### 3.代码实现：

**C++版本：**
```C++
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        ret = INT_MIN;
        dfs(root);
        return ret;
    }

private:
    int ret;
    int dfs(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int left = max(0, dfs(root->left));
        int right = max(0, dfs(root->right));
        ret = max(ret, left + right + root->val);
        return root->val + max(left, right);
    }
};

```

**Python版本：**

```Python
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        self.result = float(-inf)
        self.dfs(root)
        return self.result
    
    def dfs(self, root):
        if not root:
            return 0
        left = max(0, self.dfs(root.left))
        right = max(0, self.dfs(root.right))
        current_value = root.val + left + right
        self.result = max(self.result, current_value)
        return max(left, right) + root.val

```
