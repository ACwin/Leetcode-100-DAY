### 1.题目描述

 **[剑指 Offer II 004. 只出现一次的数字](https://leetcode-cn.com/problems/WGki4K/)** 
 
 给你一个整数数组 nums ，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次 。请你找出并返回那个只出现了一次的元素。

 

**示例 1：**
```
输入：nums = [2,2,3,2]
输出：3
```
**示例 2**：
```
输入：nums = [0,1,0,1,0,1,100]
输出：100
```
 

**提示：**

    - 1 <= nums.length <= 3 * 104
    - -231 <= nums[i] <= 231 - 1
    - nums 中，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次
    
------------------------------

### 2.思路分析：

1. 由于数组中的元素都在 int（即 32 位整数）范围内，因此我们可以依次计算答案的每一个二进制位是 0还是 1；
其中**bitSums[i]:** 用来保存数组nums中所有整数的二进制形式的第i个数位之和。

2. 其次如果一个数字出现3次，它的二进制每一位也出现的3次。如果把所有出现三次的数字的二进制的每一位都分别加起来，那么每一位都能被3整除。
3. 对于数组中的每一个元素 x，我们使用位运算 **(x >> i) & 1** 得到 x的第 i个二进制位，并将它们相加再对 3取余，得到的结果一定为0或 1，即为答案的第 i 个二进制位。


使用 **与运算** ，可获取二进制数字 num 的最右一位 n1:
- **n1=num&i**

配合 **无符号右移**操作 ，可获取 num所有位的值 (即 n1 ~ n32：)

- **num=num>>>1**



### 3.代码实现：


**Java版本：**

```Java
class Solution {
    public int singleNumber(int[] nums) {
        int[] bigSums = new int[32];
        for(int num : nums){
            for(int i = 0; i < 32; i++){
                bigSums[i] += (num >> (31-i)) & 1;


            }
        }
        int result = 0;
        for(int i = 0; i < 32; i++){
            result = (result << 1 ) + bigSums[i] % 3;

        }
        return result;

    }
}
```

**Python版本：**

**注意：**
>由于 Python 的存储负数的特殊性，需要先将0-32位取反（即 res ^ 0xffffffff ），再将所有位取反(即 ~ )
>两个组合操作实质上是将数字 32 以上位取反，0-32位不变.

```Python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        counts = [0] * 32
        for num in nums:
            for j in range(32):
                counts[j] += num & 1
                num >>= 1
        res, m = 0, 3
        for i in range(32):
            res <<= 1
            res |= counts[31 - i] % m
        return res if counts[31] % m == 0 else ~(res ^ 0xffffffff)

```
**复杂度分析：**

   - 时间复杂度 O(N)： 其中 N 位数组 nums的长度；遍历数组占用 O(N)，每轮中的常数个位运算操作占用 O(1)。
   - 空间复杂度 O(1)： 数组 counts长度恒为 32，占用常数大小的额外空间。


