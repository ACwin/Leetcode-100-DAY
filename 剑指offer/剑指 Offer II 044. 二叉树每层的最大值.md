### 1.题目描述


 **[剑指 Offer II 044. 二叉树每层的最大值](https://leetcode-cn.com/problems/hPov7L/)** 
 
给定一棵二叉树的根节点 root ，请找出该二叉树中每一层的最大值。

 

**示例1：**
```
输入: root = [1,3,2,5,3,null,9]
输出: [1,3,9]
解释:
          1
         / \
        3   2
       / \   \  
      5   3   9 
```

**示例2：**
```
输入: root = [1,2,3]
输出: [1,3]
解释:
          1
         / \
        2   3
```

**示例3：**

```
输入: root = [1,null,2]
输出: [1,2]
解释:      
           1 
            \
             2     
```

**提示：**

- 二叉树的节点个数的范围是 [0,104]
- -231 <= Node.val <= 231 - 1

 

**注意:** 本题与lc 515 题相同： https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/

-------------

### 2.解题思路：
> 层序遍历，取每一层的最大值

### 3.代码实现：

**JAVA版本：**
```Java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
		List<Integer> retVal = new ArrayList<Integer>();
		Queue<TreeNode> tmpQueue = new LinkedList<TreeNode>();
		if (root != null) tmpQueue.add(root);
		
		while (tmpQueue.size() != 0){
			int size = tmpQueue.size();
			List<Integer> lvlVals = new ArrayList<Integer>();
			for (int index = 0; index < size; index++){
				TreeNode node = tmpQueue.poll();
				lvlVals.add(node.val);
				if (node.left != null) tmpQueue.add(node.left);
				if (node.right != null) tmpQueue.add(node.right);
			}
			retVal.add(Collections.max(lvlVals));
		}
        
        return retVal;
    }
}

```
**C++版本：**
```C++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        vector<int> result;
        while (!que.empty()) {
            int size = que.size();
            int maxValue = INT_MIN; // 取每一层的最大值
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                maxValue = node->val > maxValue ? node->val : maxValue;
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(maxValue); // 把最大值放进数组
        }
        return result;
    }
};

```

**Python版本：**

```Python
class Solution:
    def largestValues(self, root: TreeNode) -> List[int]:
        if root is None:
            return []
        queue = [root]
        out_list = []
        while queue:
            length = len(queue)
            in_list = []
            for _ in range(length):
                curnode = queue.pop(0)
                in_list.append(curnode.val)
                if curnode.left: queue.append(curnode.left)
                if curnode.right: queue.append(curnode.right)
            out_list.append(max(in_list))
        return out_list

```
