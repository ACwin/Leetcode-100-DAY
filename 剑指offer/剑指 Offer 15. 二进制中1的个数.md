
### 1.题目描述

 **[剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)** 
 
 编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为 汉明重量).）。

 

**提示：**

> 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
> 在 Java 中，编译器使用 二进制补码 记法来表示有符号整数。因此，在上面的 示例 3 中，输入表示有符号整数 -3。

 

**示例 1：**
```
输入：n = 11 (控制台输入 00000000000000000000000000001011)
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```

**提示：**

- 输入必须是长度为 32 的 二进制串 。

 

注意：本题与lc 191 题相同：https://leetcode-cn.com/problems/number-of-1-bits/

--------------------------

### 2.思路分析：

**方法一：逐位判断**

    根据 与运算 定义，设二进制数字 n ，则有：
        - 若 n&1=0，则 n 二进制 最右一位 为 0 ；
        - 若 n&1=1，则 n 二进制 最右一位 为 1 。
    根据以上特点，考虑以下 循环判断 ：
        1. 判断 n最右一位是否为 1 ，根据结果计数。
        2. 将 n 右移一位（本题要求把数字 n看作无符号数，因此使用 无符号右移 操作）。


**方法二：巧用 n&(n−1)**

    - (n−1)解析： 二进制数字 nnn 最右边的 111 变成 000 ，此 111 右边的 000 都变成 111 。
    - n&(n−1)解析： 二进制数字 nnn 最右边的 111 变成 000 ，其余不变。

![图片](https://user-images.githubusercontent.com/42907149/141769894-d44e514f-f503-4313-9138-14b6c23160c1.png)

**算法流程：**

1.初始化数量统计变量 res
2.循环消去最右边的 111 ：当 n=0时跳出
- res += 1 ： 统计变量加 111 ；
- n &= n - 1 ： 消去数字 n 最右边的 1
3.返回统计数量 res


### 3.代码实现：

**C++版本：**

```C++
//方法一：逐位判断
class Solution {
public:
    int hammingWeight(uint32_t n) {
        //初始化数量统计变量 res=0 循环逐位判断： 当 n=0时跳出
        int res = 0;
        while(n != 0){
            //res += n & 1 ： 若 n&1=1 则统计数 res 加一
            res += n & 1;
             //n >>= 1 ： 将二进制数字 n 无符号右移一位返回统计数量 res
            n >>= 1;
        }
        return res;  
    }
};
```

**Java版本：**
```Java
//方法一：逐位判断
public class Solution {
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res += n & 1;
            n >>>= 1;
        }
        return res;
    }
}
```
**Python版本：**
```Python
//方法一：逐位判断
class Solution:
    def hammingWeight(self, n: int) -> int:
        res = 0
        while n:
            res += n & 1
            n >>= 1
        return res
```
### 复杂度分析：

- 时间复杂度 O(log2n)
- 空间复杂度 O(1)： 变量 res使用常数大小额外空间



**方法二：巧用 n&(n−1)**


**Java版本：**
```Java
#方法二：巧用 n&(n−1)
public class Solution {
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res++;
            n &= n - 1;
        }
        return res;
    }
}

```
**Python版本：**
```Python
#方法二：巧用 n&(n−1)
class Solution:
    def hammingWeight(self, n: int) -> int:
        res = 0
        while n:
            res += 1
            n &= n - 1
        return res

```

### 复杂度分析：

- 时间复杂度 O(M)： n&(n−1)操作仅有减法和与运算，占用 O(1)；设 M为二进制数字 n 中 1 的个数，则需循环 M 次（每轮消去一个 11 ），占用 O(M)。

- 空间复杂度 O(1)： 变量 res使用常数大小额外空间。









