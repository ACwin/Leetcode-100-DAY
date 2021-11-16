### 1.题目描述

 **[剑指 Offer II 048. 序列化与反序列化二叉树](https://leetcode-cn.com/problems/h54YBf/)** 
 
 > 请设计一个算法将二叉树化成一个字符串，并能将该字符串反序列化出原来二叉树算法

 ![图片](https://user-images.githubusercontent.com/42907149/141937183-6d8ed732-4300-40f0-bdef-50a729e70f1f.png)
 
**示例 2：**
```
输入：root = []
输出：[]
```
**示例 3：**
```
输入：root = [1]
输出：[1]
```
**示例 4：**
```
输入：root = [1,2]
输出：[1,2]
```

**提示：**

- 输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，也可以采用其他的方法解决这个问题。
- 树中结点数在范围 [0, 104] 内
- -1000 <= Node.val <= 1000


 注意：本题与lc 297 题相同：https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/ 




-------------

### 2.解题思路：

**先序遍历的方式序列化二叉树**

>因为若采用先序遍历，那么根节点最先被序列化到字符串中，然后是左子树，然后是右子树，这样具有递归形式。
>另外在反序列化的时候，从字符串中第一个读出来的字符肯定是根节点

光是有先序遍历得话，首先要用逗号把不同节点的值隔开。左右两棵树的遍历结果都是 "6, 6, 6, 6, 6"，
但是如果考虑把两棵树的叶子节点指向的空指针也序列化为 “#”，则左边的树序列化为 "6, 6, 6, #, #, 6, #, #, 6, #, #"，
右边的树序列化为 "6, 6, #, #, 6, 6, #, #, 6, #, #"。序列化空指针后，先序遍历序列化就可以唯一确定一棵二叉树。
![图片](https://user-images.githubusercontent.com/42907149/141944833-9599f905-5a24-4312-85a7-44a077d16a9b.png)


```Java
 public String serialize(TreeNode root) {
        if(root == null){
            return "#";

        }
        //前序：中左右
        String leftStr = serialize(root.left);
        String rightStr = serialize(root.right);
        return String.valueOf(root.val) + "," + leftStr + "," + rightStr;
        
    }
```
**二叉树反序列化**

由于序列化时以逗号为分界符，所以可以根据逗号把序列后的字符串分割成若干个字符串，每个字符串对应一个节点。
因为采用先序遍历序列化二叉树，所以分割的第一个字符串对应根节点，根据该字符串构造根节点。之后使用同样的方式构造左右子树，可以使用递归函数完成。二叉树序列化的代码如下。
```Java
  public TreeNode deserialize(String data) {
        //反序列化
        String[] nodeStrs = data.split(",");
        int [] i  = {0};
        return dfs(nodeStrs,i);
    }
    private TreeNode dfs(String[] strs,int[] i){
        String str = strs[i[0]];
        i[0]++;
        if(str.equals("#")){
            return null;
        }
        TreeNode node = new TreeNode(Integer.valueOf(str));
        node.left = dfs(strs,i);
        node.right = dfs(strs,i);
        return node;
    }
```


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
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "#";

        }
        //前序：中左右
        String leftStr = serialize(root.left);
        String rightStr = serialize(root.right);
        return String.valueOf(root.val) + "," + leftStr + "," + rightStr;
        
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        //反序列化
        String[] nodeStrs = data.split(",");
        int [] i  = {0};
        return dfs(nodeStrs,i);
    }
    private TreeNode dfs(String[] strs,int[] i){
        String str = strs[i[0]];
        i[0]++;
        if(str.equals("#")){
            return null;
        }
        TreeNode node = new TreeNode(Integer.valueOf(str));
        node.left = dfs(strs,i);
        node.right = dfs(strs,i);
        return node;
    }
}

```
**C++版本：**
```C++
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (root == nullptr) {
            return "#";
        }
        string leftStr = serialize(root->left);
        string rightStr = serialize(root->right);
        return to_string(root->val) + "," + leftStr + "," + rightStr;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        // 分割字符串
        vector<string> dataArray{""};
        for (auto& ch : data) {
            if (ch == ',') {
                dataArray.push_back("");
            } 
            else {
                dataArray.back().push_back(ch);
            }
        }
        int i = 0;
        return dfs(dataArray, i);
    }

private:
    TreeNode* dfs(vector<string>& strs, int & i) {
        string str = strs[i];
        i++;
        if (str == "#") {
            return nullptr;
        }

        TreeNode* node = new TreeNode(stoi(str));
        node->left = dfs(strs, i);
        node->right = dfs(strs, i);
        return node;
    }
};
```
