### 1.题目描述

 **[剑指 Offer II 003. 前 n 个数字二进制中 1 的个数](https://leetcode-cn.com/problems/w3tCBm/)** 
 
 ![image](https://user-images.githubusercontent.com/42907149/141676329-ac03ae8c-f7cc-4056-8126-536e431018cf.png)
 
 ### 2.解题思路

**动态规划 +位运算**

对于所有的数字，只有奇数和偶数两种：

1. 奇数：二进制表示中，奇数一定比前面那个偶数多一个 1，因为多的就是最低位的 1。
2. 偶数：二进制表示中，偶数中 1 的个数一定和除以 2 之后的那个数一样多。因为最低位是 0，除以 2 就是右移一位，也就是把那个 0 抹掉而已，所以 1 的个数是不变的。

所以我们可以得到如下的状态转移方程：
- dp[i] = dp[i-1]，当i为奇数
- dp[i] = dp[i/2]，当i为偶数

上面的方程还可进一步合并为：
**dp[i] = dp[i/2] + i % 2**

通过位运算进一步优化：

- i / 2 可以通过 i >> 1 得到；

- i % 2 可以通过 i & 1 得到；


### 代码实现

**JAVA版本**
```Java

class Solution {
public:
    vector<int> countBits(int n) 
    {
        vector<int> res(n + 1, 0);
        for (int i = 0; i <= n; i++)
        {
            res[i] = res[i >> 1] + (i & 1);
        }
        return res;
    }
};
```

**C++版本**
```C++
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> res(n + 1);
        for (int i = 1; i <= n; i++) {
            res[i] = res[i & (i - 1)] + 1;
        }
        return res;
    }
};
```

**Python版本**
```Python
class Solution:
    def countBits(self, n: int) -> List[int]:
        res = [0]
        for i in range(1, n + 1):
            res.append(res[i & (i - 1)] + 1)
        return res


```
