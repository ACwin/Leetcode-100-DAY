### 1.题目描述

 **[剑指 Offer II 002. 二进制加法](https://leetcode-cn.com/problems/JFETK5/)** 

![image](https://user-images.githubusercontent.com/42907149/141610804-b2b1556f-9e46-4ca7-b192-c97f92e21ad0.png)

注意：本题与lc 67 题相同：https://leetcode-cn.com/problems/add-binary/

--------------------------------------

### 2.解题思路：

模拟法
> 二进制的进位规则是「逢二进一」，即当求和结果>=2 时，需要向前进位。
**注意:**

    - 二进制数字是字符串形式，不可以转化成 int 型，因为可能溢出；
    - 两个「加数」的字符串长度可能不同；
    - carry 不为 0 需进位；
    - 向结果字符串 result 拼接的顺序是向后拼接，返回时需要把  result 反转 。


### 3.代码实现：

**JAVA版本：**

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder result = new StringBuilder(); // 返回结果
        int i = a.length() - 1; 
        int j = b.length() - 1; 
        int carry = 0; // 进位
        while (i >= 0 || j >= 0 || carry != 0) { // a 没遍历完，或 b 没遍历完，或进位不为 0
            int digitA = i >= 0 ? a.charAt(i) - '0' : 0; // 当前 a 的取值
            int digitB = j >= 0 ? b.charAt(j) - '0' : 0; // 当前 b 的取值
            int sum = digitA + digitB + carry; // 当前位置相加的结果
            carry = sum >= 2 ? 1 : 0; // 是否有进位
            sum = sum >= 2 ? sum - 2 : sum; // 去除进位后留下的数字
            res.append(sum); // 把去除进位后留下的数字拼接到结果中
            i --;  // 遍历到 a 的位置向左移动
            j --;  // 遍历到 b 的位置向左移动
        }
        return result.reverse().toString(); // 把结果反转并返回
    }
}
```
**C++版本：**
```C++
class Solution {
public:
    string addBinary(string a, string b) {
        string res;
        int carry = 0;
        int i = a.size() - 1;
        int j = b.size() - 1;
        while (i >= 0 || j >= 0 || carry != 0) {
            int digitA = i >= 0 ? a[i] - '0' : 0;
            int digitB = j >= 0 ? b[j] - '0' : 0;
            int sum = digitA + digitB + carry;
            carry = sum >= 2 ? 1 : 0;
            sum = sum >= 2 ? sum - 2 : sum;
            res += to_string(sum);
            i --;
            j --;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};

```

**Python版本：**
```Python
class Solution(object):
    def addBinary(self, a, b):
        res = ""
        carry = 0
        i = len(a) - 1
        j = len(b) - 1
        while i >= 0 or j >= 0 or carry != 0:
            digitA = int(a[i]) if i >= 0 else 0
            digitB = int(b[j]) if j >= 0 else 0
            sum = digitA + digitB + carry
            carry = 1 if sum >= 2 else 0
            sum = sum - 2 if sum >= 2 else sum
            res += str(sum)
            i -= 1
            j -= 1
        return res[::-1]
```

**复杂度分析：**

- 时间复杂度：O(max(M,N))，M 和 N 分别是字符串 a 和 b 的长度；
- 空间复杂度：O(1)，只使用了常数的空间。
