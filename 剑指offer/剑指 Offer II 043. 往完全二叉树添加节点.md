### 1.题目描述

 **[剑指 Offer II 043. 往完全二叉树添加节点](https://leetcode-cn.com/problems/NaqhDT/)** 
 
![图片](https://user-images.githubusercontent.com/42907149/141955157-21c469b7-9922-4da5-90e9-907413969e19.png)

 

**注意：**  本题与lc 919 题相同： https://leetcode-cn.com/problems/complete-binary-tree-inserter/




-------------

### 2.解题思路：

> 用队列层序遍历辅助插入。队列中存的都是从上到下，节点缺左或右节点的树节点。

### 3.代码实现：

**JAVA版本：**
```Java
class CBTInserter {    
    private Queue<TreeNode> queue;
    private TreeNode root;

    public CBTInserter(TreeNode root) {
        this.root = root;
        
        queue = new LinkedList<>();
        queue.offer(root);
        //在初始化树时就层序遍历到第一个没有左或右子树的节点，即为待插入位置的父节点，在队列头部
        while (queue.peek().left != null && queue.peek().right != null) {
            TreeNode node = queue.poll();
            queue.offer(node.left);
            queue.offer(node.right);
        }
    }
    
    public int insert(int v) {
        //队列头部节点即为待插入位置的父节点
        TreeNode parent = queue.peek();
        TreeNode node = new TreeNode(v);
        //插入左子树，父节点仍无右子树，父节点不变
        //插入右子树，左右子树入列，并将该父节点出列，待插入位置更改为下一个
        if (parent.left == null) {
            parent.left = node;
        } else {
            parent.right = node;
            
            queue.poll();
            queue.offer(parent.left);
            queue.offer(parent.right);
        }
        
        return parent.val;
    }
    
    public TreeNode get_root() {
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
class CBTInserter {
private:
    queue<TreeNode*> que;//记录当前左右子树没有满的TreeNode节点队列
    TreeNode *root;//用来记录供get_root()函数使用的root节点地址   
public:
   CBTInserter(TreeNode* root) {
        this->root = root;
        que.push(root);
         while(que.front()->left && que.front()->right){
            que.push(que.front()->left);
            que.push(que.front()->right);
            que.pop();
        }
    }

   int insert(int v) {
        TreeNode *newNode = new TreeNode(v);
        int res = que.front()->val;//先记录队头元素的值
        if(que.front()->left){
            que.front()->right = newNode;//插到右边，该节点有左右了
            que.push(que.front()->left);//把该节点左右入队，并删除队头
            que.push(que.front()->right);
            que.pop();
        }else
            que.front()->left = newNode;
        return res;
    }
    
    TreeNode* get_root() {
        return this->root;
    }
};
```



**复杂度分析：**

- 时间复杂度： O(n),队列中存的节点数为 O(n)
- 空间复杂度：O(n)
