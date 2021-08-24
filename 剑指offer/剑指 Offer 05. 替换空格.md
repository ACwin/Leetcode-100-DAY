请实现一个函数，把字符串 s 中的每个空格替换成"%20"

示例 1

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

限制：

* 0 <= s 的长度 <= 10000

---

## 解题思路：

1. 先遍历一遍字符串得出字符串中空格总数
2. 每换一次空格长度都会加2，所以替换后的新字符串长度 = 旧字符串长度 + 2 * 空格数目
3. 从字符串后面开始复制和替换用p1，p2。p1指向字符串末端，p2指向替换后的字符串末尾
4. p1,p2走到相同位置时，结束循环




```C++
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0, len = s.size();
        for (char c : s) {
            if (c == ' ') count++;
        }
        s.resize(len + 2 * count);
        for(int i = len - 1, j = s.size() - 1; i < j; i--, j--) {
            if (s[i] != ' ')
                s[j] = s[i];
            else {
                s[j - 2] = '%';
                s[j - 1] = '2';
                s[j] = '0';
                j -= 2;
            }
        }
        return s;
    }
};
```









