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

使用 **与运算** ，可获取二进制数字 num 的最右一位 n1:
- **n1=num&i**

配合 **无符号右移**操作 ，可获取 num所有位的值 (即 n1 ~ n32：)

- **num=num>>>1**

建立一个长度为 32 的数组 counts，通过以上方法可记录所有数字的各二进制位的 1的出现次数。


### 3.代码实现：


**Java版本：**

```Java
class Solution {
    public int singleNumber(int[] nums) {
        int[] counts = new int[32];
        for(int i = 0; i < nums.length; i++) {
            for(int j = 0; j < 32; j++) {
                counts[j] += nums[i] & 1; // 更新第 j 位
                nums[i] >>>= 1; // 第 j 位 --> 第 j + 1 位
            }
        }

        int res = 0, m = 3;
        for(int i = 0; i < 32; i++) {
            res <<= 1;// 左移 1 位
            res |= counts[31 - i] % m;
        }
        return res;
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

   - 时间复杂度 O(N)： 其中 NNN 位数组 numsnumsnums 的长度；遍历数组占用 O(N)O(N)O(N) ，每轮中的常数个位运算操作占用 O(1)O(1)O(1) 。
   - 空间复杂度 O(1)： 数组 countscountscounts 长度恒为 323232 ，占用常数大小的额外空间。


