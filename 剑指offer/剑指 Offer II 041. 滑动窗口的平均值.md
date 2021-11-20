### 1.题目描述

 **[剑指 Offer II 041. 滑动窗口的平均值](https://leetcode-cn.com/problems/qIsx9U/)** 
 

 给定一个整数数据流和一个窗口大小，根据该滑动窗口的大小，计算滑动窗口里所有数字的平均值。

**实现 MovingAverage 类：**

- MovingAverage(int size) 用窗口大小 size 初始化对象。
- double next(int val) 成员函数 next 每次调用的时候都会往滑动窗口增加一个整数，请计算并返回数据流中最后 size 个值的移动平均值，即滑动窗口里所有数字的平均值。


 

**示例:**
```
输入：
inputs = ["MovingAverage", "next", "next", "next", "next"]
inputs = [[3], [1], [10], [3], [5]]
输出：
[null, 1.0, 5.5, 4.66667, 6.0]

解释：
MovingAverage movingAverage = new MovingAverage(3);
movingAverage.next(1); // 返回 1.0 = 1 / 1
movingAverage.next(10); // 返回 5.5 = (1 + 10) / 2
movingAverage.next(3); // 返回 4.66667 = (1 + 10 + 3) / 3
movingAverage.next(5); // 返回 6.0 = (10 + 3 + 5) / 3
```
-------------

### 2.解题思路：

>用队列来实现滑动窗口，队列长度就是窗口大小。

### 3.代码实现：

**JAVA版本：**
```Java
class MovingAverage {

    /** Initialize your data structure here. */
    private int length;//capacity
    private Queue<Integer> nums;
    private double sum;

    public MovingAverage(int size) {
        length = size;
        nums = new LinkedList<>();
    }
    
    public double next(int val) {
        //用队列来实现滑动窗口，队列长度就是窗口大小
        nums.offer(val);
        sum += val;
        if(nums.size() > length){

            sum-= nums.poll();

        }
        return sum/nums.size();
    }
}
```
**C++版本：**
```C++
class MovingAverage {
private:
    int len  = 0;
    queue<int> nums;
    double sum = 0;
public:

    MovingAverage(int size) {
        len = size;

    }
    
    double next(int val) {
        nums.push(val);
        sum+=val;
        if(nums.size() > len){
            sum-=nums.front();
            nums.pop();

        }
        return sum/nums.size();

    }
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage* obj = new MovingAverage(size);
 * double param_1 = obj->next(val);
 */
```

**Python版本：**

```Python
class MovingAverage:

    def __init__(self, size: int):
        self.size = size
        self.queue = deque()
        self.sum = 0

    def next(self, val: int) -> float:
        self.sum += val
        self.queue.append(val)
        if len(self.queue) > self.size:
            self.sum -= self.queue.popleft()
        return self.sum / len(self.queue)
```

**复杂度分析：**

- 时间复杂度：O(1)
