### 1.题目描述

 **[剑指 Offer II 050. 向下的路径节点之和](https://leetcode-cn.com/problems/6eUYwP/)** 
 
给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是**路径**方向必须是向下的（只能从父节点到子节点）。

**示例 1：**

![图片](https://user-images.githubusercontent.com/42907149/144371526-c601b21e-8c1d-4ea3-8a09-9d903d4fe3fd.png)

```
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```


**注意**：本题与lc 437 题相同：https://leetcode-cn.com/problems/path-sum-iii/

-------------

### 2.解题思路：

> 双重递归：先调用dfs函数从root开始查找路径，再调用pathsum函数到root左右子树开始查找

二叉树路径的问题大致可以分为两类：

**1、自顶向下：**

> 从某一个节点(不一定是根节点)，从上向下寻找路径，到某一个节点(不一定是叶节点)结束

**dfs模板：**

**一般路径：**
```C++
vector<vector<int>>res;
void dfs(TreeNode*root,vector<int>path)
{
    if(!root) return;  //根节点为空直接返回
    path.push_back(root->val);  //作出选择
    if(!root->left && !root->right) //如果到叶节点  
    {
        res.push_back(path);
        return;
    }
    dfs(root->left,path);  //继续递归
    dfs(root->right,path);
}
```
**给定和的路径：**
```c++
void dfs(TreeNode*root, int sum, vector<int> path)
{
    if (!root)
        return;
    sum -= root->val;
    path.push_back(root->val);
    if (!root->left && !root->right && sum == 0)
    {
        res.push_back(path);
        return;
    }
    dfs(root->left, sum, path);
    dfs(root->right, sum, path);
}

```

具体题目如下：

* **[257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)** 
* **[面试题 04.12. 求和路径](https://leetcode-cn.com/problems/paths-with-sum-lcci/)** 
* **[112. 路径总和](https://leetcode-cn.com/problems/path-sum/)** 
* **[113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)** 
* **[437. 路径总和](https://leetcode-cn.com/problems/path-sum-iii/)** 
* **[988. 从叶结点开始的最小字符串](https://leetcode-cn.com/problems/smallest-string-starting-from-leaf/)** 


而继续细分的话还可以分成一般路径与给定和的路径

**2、非自顶向下：**

**模板：**

```c++
int res=0;
int maxPath(TreeNode *root) //以root为路径起始点的最长路径
{
    if (!root)
        return 0;
    int left=maxPath(root->left);
    int right=maxPath(root->right);
    res = max(res, left + right + root->val); //更新全局变量  
    return max(left, right);   //返回左右路径较长者
}


```
* **[124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)** 
* **[687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)** 
* **[543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)** 


这类题型DFS注意点：

* 1、left,right代表的含义要根据题目所求设置，比如最长路径、最大路径和等等

* 2、全局变量res的初值设置是0还是INT_MIN要看题目节点是否存在负值,如果存在就用INT_MIN，否则就是0

* 3、注意两点之间路径为1，因此一个点是不能构成路径的


### 3.代码实现：

**JAVA版本：**
```Java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        HashMap<Long, Integer> prefix = new HashMap<>();
        prefix.put(0L, 1);
        return dfs(root, prefix, 0, targetSum);
    }

    public int dfs(TreeNode root, Map<Long, Integer> prefix, long curr, int targetSum) {
        if (root == null) {
            return 0;
        }

        int ret = 0;
        curr += root.val;

        ret = prefix.getOrDefault(curr - targetSum, 0);
        prefix.put(curr, prefix.getOrDefault(curr, 0) + 1);
        ret += dfs(root.left, prefix, curr, targetSum);
        ret += dfs(root.right, prefix, curr, targetSum);
        prefix.put(curr, prefix.getOrDefault(curr, 0) - 1);

        return ret;
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
    map<int, int> mp;
    int target, ans;
    int pathSum(TreeNode* root, int targetSum) {
        target = targetSum;
        mp[0] = 1;
        dfs(root, 0);
        return ans;
    }

    void dfs(TreeNode* node, int sum) {
        if (node == NULL)
            return;
        sum += node->val;
        ans += mp[sum - target];
        if (mp.find(sum) != mp.end()) mp[sum]++;
        else mp[sum] = 1;
        dfs(node->left, sum);
        dfs(node->right, sum);
        mp[sum]--;
    }
};
```

**Python版本：**

```Python
class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        prefix = collections.defaultdict(int)
        prefix[0] = 1

        def dfs(root, curr):
            if not root:
                return 0
            
            ret = 0
            curr += root.val
            ret += prefix[curr - targetSum]
            prefix[curr] += 1
            ret += dfs(root.left, curr)
            ret += dfs(root.right, curr)
            prefix[curr] -= 1

            return ret

        return dfs(root, 0)

```

**复杂度分析：**

- 时间复杂度：O(N)，其中 N 为二叉树中节点的个数。利用前缀和只需遍历一次二叉树即可。
- 空间复杂度：O(N)
