### 1.题目描述

 **[剑指 Offer II 005. 单词长度的最大乘积](https://leetcode-cn.com/problems/aseY1I/)** 
 
 给定一个字符串数组 words，请计算当两个字符串 words[i] 和 words[j] 不包含相同字符时，它们长度的乘积的最大值。假设字符串中只包含英语的小写字母。如果没有不包含相同字符的一对字符串，返回 0。

 

**示例 1:**
```
输入: words = ["abcw","baz","foo","bar","fxyz","abcdef"]
输出: 16 
解释: 这两个单词为 "abcw", "fxyz"。它们不包含相同字符，且长度的乘积最大。
```
**示例 2:**
```
输入: words = ["a","ab","abc","d","cd","bcd","abcd"]
输出: 4 
解释: 这两个单词为 "ab", "cd"。
```
**示例 3:**
```
输入: words = ["a","aa","aaa","aaaa"]
输出: 0 
解释: 不存在这样的两个单词。
```
 

**提示：**

- 2 <= words.length <= 1000
- 1 <= words[i].length <= 1000
- words[i] 仅包含小写字母

-----------------------------------------------

### 2.思路分析：

1. 利用二进制特性 0：false 1：true 把每个单词中的每个字符都保存起来
2. 然后利用二进制值 与 的特性， 0 & 0 = 0, 0 & 1 = 1 & 0 = 0, 1 & 1 =1 依次比较两个单词中每个字符，记录一个字母是否出现(从右向左方向)。
3. 最后比较两个字符串是否存在相同字母时，使用 flags[i] & flags[j]) == 0



### 3.代码实现

**Java版本：**

```Java
class Solution {
    public int maxProduct(String[] words) {
        int[] flags = new int[words.length];
        for(int i = 0; i < words.length; i++){
            for(char ch:words[i].toCharArray()){
                flags[i]|= 1 << (ch - 'a');

            }

        }
        int result = 0;
        for(int i = 0; i < words.length; i++){
            for(int j =  i + 1; j < words.length; j++){
                if((flags[i] & flags[j]) == 0){
                    int prod = words[i].length() * words[j].length();
                    result = Math.max(result,prod);
                }

            }

        }
        return result;

    }
}
```

**C++版本：**

```C++
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int n = words.size();
        //存储words[i]的二进制
        int states[n];
        for(int i = 0; i < n; i++){
            states[i] = 0;
            for(int j = 0; j < words[i].size(); j++){
                //words[i][j] - 'a' 代表对应哪个位
                states[i] |= (1 << (words[i][j] - 'a'));
            }
        }
        int ans = 0;
        for(int i = 0; i < n; i++){
            for(int j = i + 1; j < n; j++){
                //等于0说明两者不相等
                if((states[i] & states[j]) == 0){
                    int tmp = words[i].size() * words[j].size();
                    ans = max(ans, tmp);
                }
            }
        }
        return ans;
    }
};
```

**复杂度分析：**
- 时间复杂度为 O(nk+n^2)
- 空间复杂度为 O(n)
